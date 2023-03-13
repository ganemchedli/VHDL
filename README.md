

# VHDL

# Introduction et structure d’un programme VHDL

### Structure d’un programme VHDL

La description d’une port ET

![portET](https://user-images.githubusercontent.com/76497607/221600794-7bdc78f5-8613-4720-a477-962d04db154b.png)


```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity portET is 
		port( entree1: in STD_LOGIC ;
		      entree2: in STD_LOGIC ; 
		      sortie : out STD_LOGIC);
		end portET;
architecture Comportementale of portET is 
begin 
		sortie <= entree1 and entree2;
end Comportementale ;
```

### Description externe : l’entité

Syntaxe :

```vhdl
entity nom de l'entité is 
		Port (liste des ports );
end nom de l'entité ;
```

Ports : signaux caractérisés par un mode et un type 

- 4 mode possibles :
    - in pour les entrées,
    - out pour les sorties,
    - inout pour les signaux biderectionnels,
    - buffer pour les signaux de sortie réentrants
- 2 types de base pour les ports
    - Scalaire : STD_LOGIC
    - Vectoriel: STD_LOGIC_VECTOR

### Architecture interne

- Description interne de haut niveau et portable
    - Comportementale
        - Structures de traitements de flots de données
        - Processus séquentiels
    - Structurelle hiérarchique
        - Instanciation de blocs décrits en VHDL et description de leurs interconnexions
- Description interne de bas niveau (structurelle) à la portabilité limitée
    - Description structurelle reposant sur l’instanciation de primitives du composant ciblé
- Syntaxe (description comportementale seulement)

```vhdl
architecture nom de l'artchitecture of nom de l'entité is 
		--Signaux et constantes internes 
		--Fonctions internes
begin 
		--Traitements de flot de données
		--Description séquentielles
end nom de l'artchitecture ; 
```

# Signaux

### Définition

- Signal
    - Toute grandeur qui peut exister après le processus de synthèse et qui peut donc être visualisée sur un oscilloscope si on en interdit la simplification.
    - Signal = Grandeur physique.
- Les entrées, sorties et entrées-sorties de l'entité sont des signaux externes.
- Déclaration de signaux internes à l'architecture
    - Entre les mots clés architecture et begin avec la syntaxe :
    
    ```vhdl
    signal nomDuSignal typeDeSignal := valeurInitiale;
    ```
    

### Des signaux systématiquement typés

- A l'image de ce qui existe en C, C++, java...
- Typage fort
    - Contrôle strict.
    - Imposant des fonctions de conversion ou de surcharge d'opérateurs pour passer d'un type à un autre.

### Types

- bit
    - 2 valeurs: '0' ou '1'
    - Peu utilisé car insuffisant pour la synthèse et la simulation quand on cible des CPLD, FPGA, SOC...
- boolean
    - 2 valeurs possibles : true ou false
    - Type de référence pour les structures conditionnelles.
- STD_LOGIC
    - 3 valeurs ayant un sens physique
        - '0', '1' et 'Z' pour la haute impédance.
    - 6 valeurs utiles pour la simulation voire la description
        - 'U' pour non initialisé,
        - 'X' pour un résultat inconnu,
        - 'L' pour un niveau bas probable,
        - 'H' pour un niveau haut probable,
        - 'W' niveau non qualifiable par 'L' ou 'H',
        - ‘-’ sans importance.

### Types logiques vectoriels

- Assimilables à des tableaux d'éléments d'un type scalaire.
- STD_LOGIC_VECTOR = vecteur de STD_LOGIC
    - a de type STD_LOGIC_VECTOR (3 downto 0)
        - a(3) désigne le poids fort
        - a(0) est le poids faible
    
    ![a](https://user-images.githubusercontent.com/76497607/221600940-d44f150b-cf53-4f11-9241-4a7e29edb0f2.jpg)

    
    - b de type STD_LOGIC_VECTOR (1 to 4)
        - b(1) est le poids fort et b(4) le poids faible
        
     ![b](https://user-images.githubusercontent.com/76497607/221600961-50841894-332a-4cca-8757-3f44116483e4.jpg)


> Note 1: le poids fort est toujours à gauche.
> 

> Note 2: les signaux représentant des données numériques codées en binaire sur N bits doivent être déclarés sous la forme de STD_LOGIC_VECTOR (N-1 downto 0).
> 
- SIGNED et UNSIGNED
    - Ce sont des vecteurs de STD_LOGIC utilisables comme des STD_LOGIC_VECTOR et pour lesquels les opérations arithmétiques sont définies pour des représentations signées et non signées dans le paquetage NUMERIC_STD

### Types mathématiques

- INTEGER
    - Type entier de référence (32 bits par défaut et limité à 32 bits sur certains simulateurs)
    - Fonctions de conversions disponibles pour passer des INTEGER aux SIGNED ou UNSIGNED et réciproquement
    - Mot clé range
        - Permet de spécifier l'intervalle des valeurs d'un INTEGER au moment de sa déclaration.
        - Exemple:
            
            ```vhdl
            b: integer range 0 to 20;  - - b est un entier limité à l'intervalle [0;20]
            ```
            
    - NATURAL (sous-types d’INTEGER)
        - Entier positif ou nul
    - POSITIVE (sous-types d’INTEGER)
        - Entier positif
    - REAL
        - Type réel en virgule flottante simple précision à la norme IEEE754
    
    ### Types caractères et chaine de caractères
    
    - CHAR
    - STRING
    
    > Note : juste cités pour information
    > 
    
    ## Déclaration de signaux
    
    ### Signaux externes
    
    - Signaux externes = Ports(déja vus)
    
    ### Signaux internes à l’architecture
    
    ```vhdl
    architecture Comportementale of exemplesSyntaxe is 
    	signal A : STD_LOGIC;
    	signal B, C, D, E, F, G : STD_LOGIC;   --déclaration de plusieurs signaux
    	signal retenue : STD_LOGIC := '1';     --affectation d'un valeur à un signal 
    	signal compteurH : STD_LOGIC_VECTOR(9 downto 0);
    	signal compteurV : STD_LOGIC_VECTOR(9 downto 0) := (others => '0'); --Valeur initiale : "0000000000"
    	signal operande1, operande2 :STD_LOGIC_VECTOR(3 downto 0) :="0000"; --valeur initiale : "0000"
    	signal afficheur7segments : STD_LOGIC_VECTOR(1 to 7);  --cette representation n'est pas adaptée pour les calculs arithmétiques
    	signal X, Y : INTEGER range 0 to 1023; --Déclaration d'un vecteur de signaux logiques (range permet du logiciel de synthèse de limiter le nombre de bits à associer à l'entier
    begin 
    ```
    
    > En VHDL, les identificateurs(signaux, variables, process, fonctions, entités, architecture) doivent: -commencer obligatoirement par une lettre -peuvent contenir des lettres, chiffres et _(underscore) Note : VHDL n’est pas sensible à la casse (ne différencie pas minuscules et majuscules
    > 
    
    ### Défintion de constantes
    
    ```vhdl
    architecture Comportementale of decodeur7segments is 
    	signal afficheur7segments : STD_LOGIC_VECTOR(1 to 7);
    	constant CHIFFRE_1 : STD_LOGIC_VECTOR(1 to 7) :="0110000" ;
    	constant CHIFFRE_2 : STD_LOGIC_VECTOR(1 to 7) :="1101101";
    	constant CHIFFRE_3 : STD_LOGIC_VECTOR(1 to 7) :="1111001"
    begin
    ```
    
    ### Opérateurs en VHDL
    
    Opérateur d’affectation pour les signaux: < = 
    
    nom_du_signal < = expression
    
    Sous réserve qu’ils soient définis pour les types de signaux utilisés, expression peut étre écrite avec les opérateur suivants et à partir du résultat de fonctions en VHDL
    
    - Des opérateurs logiques
        - and, or ,xor (ou exclusif)
        - not (complément)
        - nand, nor
    - Des opérateurs arithmétiques (pour les types INTEGER, SIGNED, UNSIGNED)
        - +, -, *, /, mod(modulo), rem(reste)
    - Un opérateur de concaténation (pour les STD_LOGIC, STD_LOGIC_VECTOR, CHAR, STRING)
        - &
    
    ### Exemples d’affectation
    
    ```vhdl
    architecture Comportementale of exemplesSyntaxe is 
    	signal A, B, C, D, E, F, G : STD_LOGIC ;
    	signal entrees:STD_LOGIC_VECTOR(3 downto 0);
    	signal afficheur7segments : STD_LOGIC_VECTOR(1 to 7); 
    	signal opCode : STD_LOGIC_VECTOR(15 to 0);
    	signal instruction : STD_LOGIC_VECTOR(4 to 0);
    	signal X, Y, P : INTEGER range -32768 to 32767;
    begin
    	c <= not(not(E3) and not(E2) and E1 and not(E0)); --équation logique en utilisant les opérateurs logiques
    	entrees <= E3 & E2 & E1 & E0; --équation écrite en utilisant l'opérateur de concaténation & 
    	A <= afficheur7Segments(1); --extraction d'un bit dans un vecteur 
    	G <= afficheur7Segments(7);
    	instruction <= opCode(15 downto 11); -- extraction d'un vecteur dans un vecteur 
      P<= X*Y; --Calcul des entiers
    end Comportementale ;
    ```
    
    # Description structurelle en VHDL
    
    Considérons le schéma suivant et donnons-en une description en VHDL
    
    - Version schématique
    
    
    
    ![1](https://user-images.githubusercontent.com/76497607/221601029-32667646-36d6-4330-8574-d88452a467ce.jpg)

    
    - Pourquoi effectuer des descriptions structurelles en VHDL ?
        - pour écrire un fichier de tests
        - pour utiliser les ressources internes du composant programmable(PLD, FPGA …) puisque cela nécessite d’etre explicite
        - pour assembler des blocs fontionnels également décrits en VHDL ou dans autre langage
    1. Etape 1 : Ajout d’une bibliothèque et d’un paquetage :
    
    AND2 et FDCE sont des primitives Xilinx → Ajout d’une bibliothèque et d’un paquetage :
    
    ```vhdl
    library UNISIM;
    use UNISIM.Vcomponents.ALL;
    ```
    
    1. Etape 2 : Descritption externe du composant créé nommé monComposant : 
    
    ```vhdl
    entity monComposant is 
    	port (a : in std_logic ; b : in std_logic ; clear : in std_logic ;
    		clk : in std_logic ; clkn : in std_logic ; c : in std_logic );
    end monComposant;
    ```
    
    1. Etape 3 : Entre les mots clés architecture et begin : 
        1. Ajout d’un signal interne à l’architecture :
        
        ```vhdl
        architecture Comportementale of monComposant is 
        	signal D_interne : std_logic;
        ```
        
        b. Description des composants internes AND2 et FDCE :
        
        ```vhdl
        component AND2 
        	port (I0 : in std_logic ;
        	      I1 : in std_logic ;
        	      O : out std_logic );
        end component;
        component FDCE 
        	generic( INIT : bit := '0');
        	port (C : in std_logic;
        	      CE : in std_logic;
        	      CLR : in std_logic;
        	      D : in std_logic;
        	      Q : out std_logic);
        end component ;
        ```
        
    2. Instanciation et connexion des composants dans l’architecture :
        
        ```vhdl
        XLX_1 : AND2
        	port map(I0 => b,
        		 I1 =>a,
        		 O => D_interne);
        XLXI_2 : FDCE
        port map(c=>clk,
        	 CE=>clkEn,
        	 CLR=>clear,
        	 D=>D_interne,
        	 Q=>c);
        ```
        
    
    ### code complet
    
    ```vhdl
    Library ieee;
    use ieee.std_logic_1164.ALL;
    use ieee.numeric_std.ALL;
    library UNISIM;
    use UNISIM.Vcomponents.ALL;
    entity monComposant is 
    	port (a : in std_logic ;
    	      b : in std_logic ;
    	      clear : in std_logic ;
    	      clk : in std_logic ;
    	      clkn : in std_logic ;
    	      c : in std_logic );
    end monComposant;
    architecture Comportementale of monComposant is 
    	signal D_interne : std_logic;
    component AND2 
    	port (I0 : in std_logic ;
    	      I1 : in std_logic ;
    	      O : out std_logic );
    end component;
    component FDCE 
    	generic( INIT : bit := '0');
    	port (C : in std_logic;
    	      CE : in std_logic;
    	      CLR : in std_logic;
    	      D : in std_logic;
    	      Q : out std_logic);
    end component ;
    begin
    XLX_1 : AND2
    	port map(I0 => b,
    		 I1 =>a,
    		 O => D_interne);
    XLXI_2 : FDCE
    port map(c=>clk,
    	     CE=>clkEn,
    	     CLR=>clear,
    	     D=>D_interne,
    	     Q=>c);
    end Comportementale;
    ```
    # Traitement de flots de données
    
    ## Domaine concurrent
    
    C’est l’espace entre les mots clés begin et end de l’architecture 
    
    Pourquoi qualifie-t-on ce domaine de concurrent ?
    
    - Structures employées concurrentes dans le temps → Calculées en meme temps
    - Traitements parallèles
    
    Conséquence
    
    - Ordre des structures sans importance
        - Exemple :
            
            ```vhdl
            A <= C or D ;  
            B <= C and F;
            ```
            
            et 
            
            ```vhdl
            B<= C and D;
            A<= C or D;
            ```
            
            conduisent à des résultats temporels identiques 
            
        
        Elément de domaine concurrent
        
        - Traitement de flot de données:
            - Affectations simples
            - Affectations conditionnelles: when … else …
            - Affectations sélectives : with … select ….
        - Processus utilisant une description séquentielle
        
        ![lo](https://user-images.githubusercontent.com/76497607/221616965-79168404-4a1d-43ad-a79a-9a975b95cda3.jpg)

        
    
    ### Rappel : Système combinatoire
    
    Un système dont les sorties dépendent uniquement des entrées à un instant t donné est qualifié de combinatoire.
    
    Système faisant correspondre un vecteur de M sorties à un vecteur de N entrées:
    
    ![circuit](https://user-images.githubusercontent.com/76497607/221616997-d4099927-2590-4d75-bceb-fb487b099f65.jpg)

    
    Un tel système peut etre représenté sous la forme d’un tableau, appelé table de vérité, explicitant les sorties en fonction des différentes combinaisons d’entrée.
    
    ## Affectation conditionnelle : structure when .. else ..
    

### Sur l’exemple de la porte ET

```vhdl
sortie <= '1' when entree1='1' and entree2='1'
	      else '0' when entree1='1' and entree2='0'
	      else '0' when entree1='0' and entree2='1'
	      else '0' when entree1='0' and entree2='0'
	      else '0';
```

> Note : la structure se termine toujours par un else fixant la sortie pour les combinaisons non explicitées ( rappel : les signaux entree1 et entree2 peuvent 9 valeurs chacun).
> 

### Optimisation

- En regroupant les résultats communs :

```vhdl
sortie <= '1' when entree1 = '1' and entree2 = '1' else 0;
```

- En vectorisant les signaux d’entrées :

```vhdl
entity portET is 
		port( entree1: in STD_LOGIC ;
		      entree2: in STD_LOGIC ; 
		      sortie : out STD_LOGIC);
end portET;
architecture Comportementale of portET is 
	signal entrees : std_logic_vector(1 to 2);
begin
	entrees <= entree1 & entree2;
	sortie <= '1' when entrees = '11' else '0' ;
end Comportementale ;
```

## Application sur un exemple

Unité arithmétique simple 

![ual](https://user-images.githubusercontent.com/76497607/221617033-a5679aa5-b70d-463a-91b4-1835990bc6a0.jpg)


- Deux opérations possibles :
    - Operation = ‘0’ → Resultat ← Operande1 + OperandeB
    - Operation = ‘1’ → Resultat ← OperandeA - OperandeB

### Code VHDL avec affectation conditonnelle

```vhdl
Library ieee;
use ieee.std_logic_1164.ALL;
use ieee.numeric_std.ALL;

entity ual is 
	port ( operandeA : in std_logic_vector(7 downto 0);
	 operandeB : in std_logic_vector(7 downto 0);
	 operation : in std_logic;
         resultat : out std_logic_vector(7 downto 0));
end ual;

architecture Comportementale of ual is 
begin 
	resultat <= std_logic_vector(signed(operandeA) + signed(operandeB)) when operation = '0',
				else   std_logic_vector(signed(operandeA) - signed(operandeB)) when operation = '1', 
				else "00000000" ;
end Comportementale ;
```

## Affectation sélective : structure with … select …

Sur l’exemple de la porte ET, après vectorisation des entrées 

```vhdl
architecture Comportementale of portET is 
	signal entrees : std_logic_vector(1 to 2);
begin 
	entrees <= entree1 & entree2 ;
	with entrees select
		sortie <= '1' when "11",
	        '0' when others;
end Comportementale;
```

> Note : la structure se termine toujours par une ligne … when others; fixant la sortie pour les combinaisons non explicitées.
> 

### Code source de l’UAL élemetaire ual.vhdl

```vhdl
Library ieee;
use ieee.std_logic_1164.ALL;
use ieee.numeric_std.ALL;

entity ual is 
	port ( operandeA : in std_logic_vector(7 downto 0);
	       operandeB : in std_logic_vector(7 downto 0);
	       operation : in std_logic;
	       resultat : out std_logic_vector(7 downto 0));
end ual;
architecture Comportementale of ual is 
begin
 with operation select 
	 	resultat <= std_logic_vector(signed(operandeA) + signed(operandeB)) when  '0',
				else   std_logic_vector(signed(operandeA) - signed(operandeB)) when  '1' ,
				(others => '0') when others ;
end Comportementale ;
```

> Note : la ligne (others ⇒ ‘0’) when others; est utile pour prévoir tous les cas d’entrées possibles, en particulier pour la simulation
>
## Domaine séquentiel

A l'intérieur du domaine concurrent, on peut effectuer des descriptions séquentielles → domaine séquentiel

- Accès à des structures puissantes et lisibles, rappelant celles du C
- Réalisation de blocs combinatoires avec une autre approche que celle du domaine concurrent mais aussi de blocs séquentiels.
- Prise en compte aisée des phénomènes synchrones.
- Ecriture de bancs de test.

Mot clé débutant la description séquentielle : process

- nom_du_process: process (liste_de_sensibilité)
    - Liste de sensibilité = liste des signaux dont le changement d'état lance le calcul du process.
    - La liste de sensibilité peut être vide→ on ôte les parenthèses. Dans ce cas, le process est exécuté sans condition.
    - Le nommage du process est facultatif.

### Structure d’un process

```vhdl
nom_du_process:
process(liste_de_sensibilité)
begin
- Déclaration des variables internes au process
- Déclaration des contantes
-- Description séquentielle <=> Prise en compte des lignes de code les unes à la suite des autres.
end process nom_du_process;
```

### Signal et Variable

Signal

- Déclaration avant le begin de l'architecture

```vhdl
architecture nom_de_1_architecture of nom_de_1_entite is signal 
	nom_signal: type_signal := valeur_initiale; 
begin
```

- Existe physiquement dans le composant sauf simplification par un processus d'optimisation.
- Affectation dans un process
    - Opérateur ≤
    - Effective seulement au moment de la sortie du process ⇒ utilisé dans un process comme entrée dans une équation, le signal a toujours pour valeur celle qu’il avait en entrant dans le process !

Variable

- Déclaration avant le begin de du process

```vhdl
process(liste_de_sensibilite)
variable nom_variable: type_variable := valeur_initiale;
begin
```

N'existe qu'à l'intérieur d'un process et disparait au moment de la sortie du process

=> la variable n'a pas de sens physique.

=> artifice utile dans le processus de description séquentielle aboutissant à un jeu d'équations après synthèse.

- Affectation dans un process
    - opérateur :=
    - Effective immédiatemment .

## Structures de programmation dans le process

Affectation simple 

- Comme dans le domaine concurrent en utilisant l’opérateur d’affectation ≤ pour les signaux
- Avec l’opérateur := pour affecter les variables

Affectation conditionnelle - Affectation sélective

- Possibles pour les signaux dans un process comme dans le domaine concurrent si les fichiers VHDL sont à la norme VHDL-2008 (ce qui n’est pas le cas par défault).

Structures conditionnelles 

- Structure

```vhdl
if .... then .... else ..... end if; (else facultatif)
```

- sur une seule ligne :

```vhdl
if condition then action_du_then ; end if;
if condition then action_du_then; else action_du_else; end if;
```

- sur plusieurs ligne

```vhdl
if condition then
		--action du then 
end if ;
if condtion then 
		--action du then 
else
		--action du else 
end if ; 
```

- Structure

```vhdl
if condition then 
		--action du then 
elsif condition_e then
		--action du elsif then 
else 
  --action du elsif else
end if;
```

- Structure case … :

```vhdl
case signal_variable is
	when val      => action_1;
	when vald to valf => action_2;
	when val1|val2|valn => action_3 ;
	when others      => action_others;
end case;

--ou version développée
case signal_variable is 
		when val  => --action du cas signal_variable = val 
		when vald to valf  => --actions du cas signal_variable compris entre vald to valf
		when others --actions pour tous les autres cas 
end case ;
```

Structures répétitives 

- Boucle for … loop

```vhdl
for index in valeur_debut to valeur_fin loop
--actions de la boucle for...loop
end loop;
```
