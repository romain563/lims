<?xml version="1.0" encoding="iso-8859-1"?>
<!-- Calcul E.coli Agro-->
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:java="http://xml.apache.org/xslt/java" xmlns:lims="xalan://lims.util.Outil" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:math="java:java.lang.Math" xmlns:outil="xalan://lims.util.Outil">
	<xsl:output method="xml" indent="yes"/>
	<xsl:template match="boites">
		<xsl:element name="calcul">
			<xsl:attribute name="estimation">false</xsl:attribute>
			<xsl:attribute name="paillasse">
				<xsl:value-of select="@paillasse"/>
			</xsl:attribute>
			<xsl:variable name="coefficient">
				<xsl:choose>
					<xsl:when test="boite[1]/@dilution = '0'">1</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-1'">0.1</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-2'">0.01</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-3'">0.001</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-4'">0.0001</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-5'">0.00001</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<xsl:variable name="min">
				<xsl:choose>
					<xsl:when test="boite[1]/@dilution = '0'">1</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-1'">10</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-2'">100</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-3'">1000</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-4'">10000</xsl:when>
					<xsl:when test="boite[1]/@dilution = '-5'">100000</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<xsl:variable name="volume">
				<xsl:value-of select="boite[1]/@volume"/>
			</xsl:variable>
			<!-- calcul maximum colonies carac boite [2] -->
			<xsl:variable name="max_dilution2">
				<xsl:choose>
					<!--Boite 2, Caract = NaN-->
					<xsl:when test="string(boite[2]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN'"> 					
						0
					</xsl:when>
					<!--Boite 2, Caract = -1-->
					<xsl:when test="boite[2]/valeurs/valeur_ex[id = '1']/h24 = -1">								
						0
					</xsl:when>
					<!--C Boite 2 différent de NaN et <100 et NC différent de NaN-->
					<xsl:when test="(string(boite[2]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') 
					and (boite[2]/valeurs/valeur_ex[id = '1']/number(h24) &lt; 150)
					and (string(boite[2]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN')">
						<xsl:value-of select="boite[2]/valeurs/valeur_ex[id = '1']/number(h24)"/>
					</xsl:when>
					<!--C Boite 2 différent de NaN et <100 et NC différent de NaN et de -1, somme de C et NC boite 2 <300-->
					<xsl:when test="(string(boite[2]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') 
					and (boite[2]/valeurs/valeur_ex[id = '1']/number(h24) &lt; 150)
					and (string(boite[2]/valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN')
					and (string(boite[2]/valeurs/valeur_ex[id = '2']/number(h24)) != '-1')
					and ((boite[2]/valeurs/valeur_ex[id = '1']/number(h24)) + (boite[2]/valeurs/valeur_ex[id = '2']/number(h24)) &lt; 300)"><!--Ajouté number-->
						<xsl:value-of select="boite[2]/valeurs/valeur_ex[id = '1']/number(h24)"/>
					</xsl:when>
					<xsl:otherwise>0</xsl:otherwise>
				</xsl:choose>
			</xsl:variable>
			<!-- test si boite 1 comptable -->
			<xsl:variable name="comptable_24">
				<xsl:choose>
					<xsl:when test="(string(boite[1]/valeurs/valeur_ex[id = '1']/h24) = '') and (string(boite[1]/valeurs/valeur_ex[id = '2']/h24) = '')"/> <!--carac vide et non carac vide-->
					<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '1']/h24) = ''"> <!--carac vide--> 
						<xsl:if test="boite[1]/valeurs/valeur_ex[id = '2']/number(h24) = -1">1</xsl:if> <!--non carac = -1-->
						<!-- + de300NC à 24h-->
					</xsl:when>
					<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '2']/h24) = ''"> <!--non carac vide-->
						<xsl:if test="boite[1]/valeurs/valeur_ex[id = '1']/number(h24) = -1">1</xsl:if> <!--carac =-1-->
						<!-- + 100C à 24h-->
					</xsl:when>
					<xsl:when test="boite[1]/valeurs/valeur_ex[id = '1']/number(h24) = -1">1</xsl:when>
					<xsl:when test="boite[1]/valeurs/valeur_ex[id = '2']/number(h24) = -1">1</xsl:when>
					<xsl:when test="boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &gt; 150">1</xsl:when>
					<xsl:when test="boite[1]/valeurs/valeur_ex[id = '2']/number(h24) &gt; 300">1</xsl:when>
					<xsl:when test="((boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) + (boite[1]/valeurs/valeur_ex[id = '2']/number(h24))) &gt; 300">1</xsl:when>
					<otherwise>
					</otherwise>
				</xsl:choose>
			</xsl:variable>
			<!-- test si valeur boite+1 suivante < valeur boite -->
			<xsl:variable name="sup">
				<xsl:for-each select="boite">
					<xsl:variable name="position">
						<xsl:value-of select="position()"/>
					</xsl:variable>
					<xsl:if test="(../boite[$position - 1]/@valeur) != ''">
						<xsl:choose>
							<xsl:when test="((string(valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') and (string(valeurs/valeur_ex[id = '1']/number(h24)) != '0')) and ((string(../boite[$position - 1]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN') or (string(../boite[$position - 1]/valeurs/valeur_ex[id = '1']/number(h24)) = '0'))">1</xsl:when>
							<xsl:when test="((string(valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN') and (string(valeurs/valeur_ex[id = '2']/number(h24)) != '0')) and ((string(../boite[$position - 1]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN') or (string(../boite[$position - 1]/valeurs/valeur_ex[id = '2']/number(h24)) = '0'))">3</xsl:when>
							<xsl:when test="(number(string(valeurs/valeur_ex[id = '1'][string(h24) != '-1']/number(h24)))) &gt; (number(string(../boite[$position - 1]/valeurs/valeur_ex[id = '1'][string(h24) != '-1']/number(h24))))">5</xsl:when>
							<xsl:when test="(number(string(valeurs/valeur_ex[id = '2'][string(h24) != '-1']/number(h24)))) &gt; (number(string(../boite[$position - 1]/valeurs/valeur_ex[id = '2'][string(h24) != '-1']/number(h24))))">7</xsl:when>
							<xsl:when test="(number(string(valeurs/valeur_ex[id = '1']/number(h24))) = -1) and (number(string(../boite[$position - 1]/valeurs/valeur_ex[id = '1']/number(h24))) != -1)">5</xsl:when>
							<xsl:when test="(number(string(valeurs/valeur_ex[id = '2']/number(h24))) = -1) and (number(string(../boite[$position - 1]/valeurs/valeur_ex[id = '2']/number(h24))) != -1)">7</xsl:when>
						</xsl:choose>
					</xsl:if>
				</xsl:for-each>
			</xsl:variable>
			<!-- test si < 10 colonies carac dans la première boite -->
			<xsl:variable name="test15_24">
				<xsl:choose>
					<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN'">
						<xsl:if test="(boite[1]/valeurs/valeur_ex[id = '1']/number(h24) != -1) and (boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &lt; 10)">1</xsl:if>
					</xsl:when>
					<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN'">1</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<!-- test si > 100 colonies carac dans la derniere boite -->
			<xsl:variable name="test100_24">
				<xsl:choose>
					<xsl:when test="string(boite[last()]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN'"/>
					<xsl:when test="((boite[last()]/valeurs/valeur_ex[id = '1']/number(h24) &gt; 150) or (boite[last()]/valeurs/valeur_ex[id = '1']/number(h24) = -1))">1</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<!-- test si > 300 colonies dans la derniere boite -->
			<xsl:variable name="test300_24">
				<xsl:choose>
					<xsl:when test="(string(boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN') and (boite[last()]/valeurs/valeur_ex[id = '2']/number(h24) &gt; 300)">1</xsl:when>
					<xsl:when test="(string(boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN') and (string(boite[last()]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') and ((boite[last()]/valeurs/valeur_ex[id = '1']/number(h24) + boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) &gt; 300)">1</xsl:when>
					<xsl:when test="string(boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) = '-1'">1</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<!-- test si > 300 colonies non carac dans la derniere boite -->
			<xsl:variable name="test300_24_non">
				<xsl:choose>
					<xsl:when test="(string(boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN') and (boite[last()]/valeurs/valeur_ex[id = '2']/number(h24) &gt; 300)">1</xsl:when>
					<xsl:when test="string(boite[last()]/valeurs/valeur_ex[id = '2']/number(h24)) = '-1'">1</xsl:when>
				</xsl:choose>
			</xsl:variable>
			<xsl:choose>
				<!-- si toutes les boites ne sont pas lues -->
				<xsl:when test="boite[@valeur = '']">
					<xsl:attribute name="cofrac">
						<xsl:value-of select="@cofrac"/>
					</xsl:attribute>
					<xsl:attribute name="expression">Avertissement : il reste des boites a lire</xsl:attribute>
				</xsl:when>
				<!-- si valeur suivante > : alerte -->
				<xsl:when test="$sup != ''">
					<xsl:attribute name="cofrac">
						<xsl:value-of select="@cofrac"/>
					</xsl:attribute>
					<xsl:attribute name="expression">Avertissement : &gt;</xsl:attribute>
				</xsl:when>
				<!-- premiere boite 0 colonies carac -->
				<xsl:when test="((string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) = '0') or (string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN'))">
					<xsl:choose>
						<!-- dernière boite illisible à 24 -->
						<xsl:when test="($test300_24_non != '')">
							<xsl:attribute name="resultat">0</xsl:attribute>
							<xsl:attribute name="expression">Flore interférente</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!-- dernière boite lisible, boite 1 : 0NC ou NaN -->
						<xsl:when test="not(($test300_24_non != '')) and (((string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = '0') or (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN')))">
							<xsl:attribute name="resultat">0</xsl:attribute>
							<xsl:attribute name="expression">&lt; <xsl:value-of select="$min div $volume"/>
							</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!-- dernière boite lisible, première boite avec colonies NC entre 0 et 300 -->
						<xsl:when test="not($test300_24_non != '') and ((((number(string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24))) &lt; 300) and (number(string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24))) &gt;= 0)) or (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN')))">
							<xsl:attribute name="resultat">0</xsl:attribute>
							<xsl:attribute name="expression">&lt; <xsl:value-of select="round((1 div $coefficient) div $volume)"/>
							</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!-- dernière boite lisible,  et pas 0 NC boite 1-->
						<!--<xsl:when test="not($test300_24_non != '') and (not(((string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = '0') or (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN'))))">
							<xsl:attribute name="resultat">0</xsl:attribute>
							<xsl:attribute name="expression">&lt; <xsl:value-of select="round((10 div $coefficient) div $volume)"/> (présence de flore interférente)</xsl:attribute>
							<xsl:attribute name="cofrac"><xsl:value-of select="@cofrac"/></xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>-->
					</xsl:choose>
				</xsl:when>
				<!-- si <15 colonies carac sur premiere boite -->
				<xsl:when test="($test15_24 != '')">
					<xsl:choose>
						<!--Boite 1 comptable-->
						<xsl:when test="($comptable_24 = '')">
							<xsl:choose>
								<!--valeur vide-->
								<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '1']/h24) = ''">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">&lt; <xsl:value-of select="round((1 div $coefficient) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--0 colonies C-->
								<xsl:when test="boite[1]/valeurs/valeur_ex[id = '1']/number(h24) = 0">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">&lt; <xsl:value-of select="round((1 div $coefficient) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--Moins 4 colonies C-->
								<xsl:when test="boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &lt; (4 )">
									<xsl:variable name="res24">
										<xsl:value-of select="((boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) div $coefficient) div $volume"/>
									</xsl:variable>
									<xsl:attribute name="resultat">
										<xsl:value-of select="round($res24 div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="expression">présence &lt; <xsl:value-of select="round((4 div $coefficient) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--Moins de 10 colonies c-->
								<xsl:otherwise>
									<xsl:variable name="res24">
										<xsl:value-of select="((boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) div $coefficient) div $volume"/>
									</xsl:variable>
									<xsl:attribute name="resultat">
										<xsl:value-of select="round($res24 div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="expression">
										<xsl:value-of select="round($res24 div $volume)"/> (Nombre estimé)</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:otherwise>
							</xsl:choose>
							<xsl:otherwise>
							</xsl:otherwise>
						</xsl:when>
						<!--Boite 1 non comptable avec colonies caractéristiques visibles-->
						<xsl:when test="($comptable_24 != '')">
							<xsl:choose>
								<!--Boite 2 non exploitable-->
								<xsl:when test="$test300_24_non != ''">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">Flore interférente<!--entre <xsl:value-of select="round(($min) div $volume)"/> et <xsl:value-of select="round(($min div (0.1)) div $volume)"/>-->
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--Boite 2 exploitable-->
								<xsl:when test="$test300_24_non = ''">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">entre <xsl:value-of select="round(($min) div $volume)"/> et <xsl:value-of select="round(($min div (0.1)) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--Utilisation de la  boite 2 si colonies caract. Attention il doit y avoir <5 col C sur dilution 2 sinon incohérent-->
								<xsl:when test="$test300_24_non = ''  and $max_dilution2 &lt; 4">
									<xsl:attribute name="resultat">
										<xsl:value-of select="round(($max_dilution2 div $coefficient) div $volume div 0.1)"/>
									</xsl:attribute>
									<xsl:attribute name="expression">présence &lt; <xsl:value-of select="round((4 div $coefficient) div $volume div 0.1)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<xsl:when test="$test300_24_non = '' and $max_dilution2 = 4">
									<xsl:attribute name="resultat">
										<xsl:value-of select="round((4 div $coefficient) div $volume div 0.1)"/>
									</xsl:attribute>
									<xsl:attribute name="expression">
										<xsl:value-of select="round((4 div $coefficient) div $volume div 0.1)"/> (Nombre estimé)</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<xsl:when test="$test300_24_non = '' and $max_dilution2 &gt; 4">
									<xsl:attribute name="resultat"/>
									<xsl:attribute name="expression">Incohérence Dil 1 avec Dil 2</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<!--A vérifier, je ne vois pas à quoi ca sert-->
								<xsl:when test="(string(boite[1]/valeurs/valeur_ex[id = '1']/h24) = '10') or (string(boite[1]/valeurs/valeur_ex[id = '1']/h24) = '10')">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">entre <xsl:value-of select="round(($min) div $volume)"/> et <xsl:value-of select="round(($min div (0.1)) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
								<xsl:when test="(string(boite[1]/valeurs/valeur_ex[id = '1']/h24) != '10') and (string(boite[1]/valeurs/valeur_ex[id = '1']/h24) != '10')">
									<xsl:attribute name="resultat">0</xsl:attribute>
									<xsl:attribute name="expression">&lt; <xsl:value-of select="round((1 div $coefficient) div $volume)"/>
									</xsl:attribute>
									<xsl:attribute name="cofrac">
										<xsl:value-of select="@cofrac"/>
									</xsl:attribute>
									<xsl:attribute name="identification">false</xsl:attribute>
								</xsl:when>
							</xsl:choose>
						</xsl:when>
					</xsl:choose>
				</xsl:when>
				<!-- derniere boite > 100 colonies carac -->
				<xsl:when test="($test100_24 != '')">
					<xsl:attribute name="resultat">
						<xsl:value-of select="round((150 div ($coefficient * 0.1)) div $volume)"/>
					</xsl:attribute>
					<xsl:attribute name="expression">&gt; <xsl:value-of select="round((150 div ($coefficient * 0.1)) div $volume)"/>
					</xsl:attribute>
					<xsl:attribute name="cofrac">
						<xsl:value-of select="@cofrac"/>
					</xsl:attribute>
					<xsl:attribute name="identification">false</xsl:attribute>
				</xsl:when>
				<!-- non compable sur derniere boite > 300 colonies -->
				<xsl:when test="($test300_24 != '')">
					<xsl:attribute name="resultat">0</xsl:attribute>
					<xsl:attribute name="expression">Flore interférente</xsl:attribute>
					<xsl:attribute name="cofrac">
						<xsl:value-of select="@cofrac"/>
					</xsl:attribute>
					<xsl:attribute name="identification">false</xsl:attribute>
				</xsl:when>
				<xsl:otherwise>
					<!-- pour 2 boites -->
					<!-- calcul maximum colonies carac boite [1] -->
					<xsl:variable name="max_dilution1">
						<xsl:choose>
							<!--Boite 1, coloçnies C = NaN-->
							<xsl:when test="string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) = 'NaN'">
								0
							</xsl:when>
							<xsl:when test="boite[1]/valeurs/valeur_ex[id = '1']/h24 = -1">
								0
							</xsl:when>
							<xsl:when test="(string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') 
							and (boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &gt; 9)
							and (boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &lt; 150)
							and (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) = 'NaN')">
								<xsl:value-of select="boite[1]/valeurs/valeur_ex[id = '1']/number(h24)"/>
							</xsl:when>
							<xsl:when test="(string(boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) != 'NaN') 
							and (boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &gt; 9)
							and (boite[1]/valeurs/valeur_ex[id = '1']/number(h24) &lt; 150)
							and (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) != 'NaN')
							and (string(boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) != '-1')
							and ((boite[1]/valeurs/valeur_ex[id = '1']/number(h24)) + (boite[1]/valeurs/valeur_ex[id = '2']/number(h24)) &lt; 300)"><!--Ajouté number-->
								<xsl:value-of select="boite[1]/valeurs/valeur_ex[id = '1']/number(h24)"/>
							</xsl:when>
							<xsl:otherwise>0</xsl:otherwise>
						</xsl:choose>
					</xsl:variable>
					<xsl:variable name="sum_colonies">
						<xsl:value-of select="$max_dilution1 + $max_dilution2"/>
					</xsl:variable>
					<xsl:variable name="coef">
						<xsl:if test="$max_dilution1 = 0 and $max_dilution2 = 0">0</xsl:if>
						<!--ajout du 10/12/20-->
						<xsl:if test="$max_dilution1 &gt;= 10 and $max_dilution2 = 0">1.1</xsl:if>
						<xsl:if test="$max_dilution1 = 0 or $max_dilution2 = 0">1</xsl:if>
						<xsl:if test="$max_dilution1 != 0 and $max_dilution2 != 0">1.1</xsl:if>
					</xsl:variable>
					<xsl:variable name="power_boite1">
						<xsl:choose>
							<xsl:when test="$max_dilution1 = 0 and $max_dilution2 = 0">1</xsl:when>
							<xsl:when test="$max_dilution1 = 0">
								<xsl:value-of select="math:pow(10, (xs:integer(boite[2]/@dilution)))"/>
							</xsl:when>
							<xsl:otherwise>
								<xsl:value-of select="math:pow(10, (xs:integer(boite[1]/@dilution)))"/>
							</xsl:otherwise>
						</xsl:choose>
					</xsl:variable>
					<!-- calcul résultat : nombre de colonies caractéristiques -->
					<xsl:variable name="res">
						<xsl:value-of select="lims:Arrondi(round(($sum_colonies) div ($volume*$coef*$power_boite1)))"/>
					</xsl:variable>
					<xsl:choose>
						<xsl:when test="$sum_colonies = 0">
							<xsl:attribute name="resultat">0</xsl:attribute>
							<xsl:attribute name="expression">entre <xsl:value-of select="round(($min) div $volume)"/> et <xsl:value-of select="round(($min div (0.1)) div $volume)"/>
							</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!--Boite 1 non comptable et moins de 4 colonies-->
						<xsl:when test="($comptable_24 != '') and ($sum_colonies &lt; 4)">
							<xsl:attribute name="resultat">
								<xsl:value-of select="$res"/>
							</xsl:attribute>
							<xsl:attribute name="expression">présence &lt; <xsl:value-of select="round(4 div ($volume*$coef*$power_boite1))"/>
							</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!--Boite 1 non comptable et plus de 10 colonies-->
						<xsl:when test="($comptable_24 != '') and ($sum_colonies &gt;= 10)">
							<xsl:attribute name="resultat">
								<xsl:value-of select="$res"/>
							</xsl:attribute>
							<xsl:attribute name="expression">
								<xsl:value-of select="lims:Format($res)"/> résultat obtenu à partir d'une seule boite</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!--Boite 1 non comptable et moins de 10 colonies-->
						<xsl:when test="($comptable_24 != '') and ($sum_colonies &lt; 10)">
							<xsl:attribute name="resultat">
								<xsl:value-of select="$res"/>
							</xsl:attribute>
							<xsl:attribute name="expression">
								<xsl:value-of select="lims:Format($res)"/> (Nombre estimé)</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
						<!--Boite 1 comptable-->
						<xsl:when test="not(($comptable_24 != '')))">
							<xsl:attribute name="resultat">
								<xsl:value-of select="$res"/>
							</xsl:attribute>
							<xsl:attribute name="expression">
								<xsl:value-of select="lims:Format($res)"/>
							</xsl:attribute>
							<xsl:attribute name="cofrac">
								<xsl:value-of select="@cofrac"/>
							</xsl:attribute>
							<xsl:attribute name="identification">false</xsl:attribute>
						</xsl:when>
					</xsl:choose>
				</xsl:otherwise>
			</xsl:choose>
			<!--			<xsl:choose>
			<!{1}** si toutes les boites ne sont pas lues **{1}>
			<xsl:when test="boite[@valeur = '']">
                </xsl:when>
<!{1}**			 si valeur suivante > : alerte 
**{1}>			<xsl:when test="$sup != ''">                    
                </xsl:when>
<!{1}**			 si aucune valeur, remise du délai à 0 
**{1}>			<xsl:when test="not(boite/valeurs/valeur_ex[h24 != '']) and not(boite/valeurs/valeur_ex[h48 != ''])">    
                    <xsl:if test="/boites/delais/delai[@position = 0]/@delai != ''">
                        <xsl:attribute name="delai_lecture_boite"><xsl:value-of select="/boites/delais/delai[@position = 0]/@delai"/></xsl:attribute>
                    </xsl:if>
                    
                    <xsl:if test="/boites/delais/delai[@position = 0]/@intervalle != ''">
                        <xsl:attribute name="delai_intervalle"><xsl:value-of select="/boites/delais/delai[@position = 0]/@intervalle"/></xsl:attribute>
                    </xsl:if>
                </xsl:when> 
<!{1}**			 si au moins une valeur x à 24H, on passe au 2e délai 
**{1}>			<xsl:when test="boite/valeurs/valeur_ex[h24 = 'x']">    
                    <xsl:if test="/boites/delais/delai[@position = 1]/@delai != ''">
                        <xsl:attribute name="delai_lecture_boite"><xsl:value-of select="/boites/delais/delai[@position = 1]/@delai"/></xsl:attribute>
                    </xsl:if>
                    
                    <xsl:if test="/boites/delais/delai[@position = 1]/@intervalle != ''">
                        <xsl:attribute name="delai_intervalle"><xsl:value-of select="/boites/delais/delai[@position = 1]/@intervalle"/></xsl:attribute>
                    </xsl:if>
                </xsl:when>
<!{1}**			 si toutes les valeurs à 24H, on va cherche le 2e delai 
**{1}>			<xsl:when test="not(boite/valeurs/valeur_ex[h24 = ''])">    
                    <xsl:if test="/boites/delais/delai[@position = 1]/@delai != ''">
                        <xsl:attribute name="delai_lecture_boite"><xsl:value-of select="/boites/delais/delai[@position = 1]/@delai"/></xsl:attribute>
                    </xsl:if>
                    
                    <xsl:if test="/boites/delais/delai[@position = 1]/@intervalle != ''">
                        <xsl:attribute name="delai_intervalle"><xsl:value-of select="/boites/delais/delai[@position = 1]/@intervalle"/></xsl:attribute>
                    </xsl:if>
                </xsl:when>
<!{1}**			 si au moins 1 lecture à 48H, on force le 2e delai 
**{1}>			<xsl:when test="boite/valeurs/valeur_ex[h48 != '']">
                    <xsl:if test="/boites/delais/delai[@position = 1]/@delai != ''">
                        <xsl:attribute name="delai_lecture_boite"><xsl:value-of select="/boites/delais/delai[@position = 1]/@delai"/></xsl:attribute>
                    </xsl:if>
                    
                    <xsl:if test="/boites/delais/delai[@position = 1]/@intervalle != ''">
                        <xsl:attribute name="delai_intervalle"><xsl:value-of select="/boites/delais/delai[@position = 1]/@intervalle"/></xsl:attribute>
                    </xsl:if>                    
                </xsl:when> 
			</xsl:choose>
-->
		</xsl:element>
	</xsl:template>
</xsl:stylesheet>