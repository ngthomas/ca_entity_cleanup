###  Entities of JP's CollectiveAccess: Typos, Alias, Redundancy & Misc Errors


##### Motivation
Through manually creating hundreds of new entities drawn from books, dance programs and moving images on Jacob's Pillow CollectiveAccess (CA), I learnt how easy it is to introduce intentional and unintentional typos in the entity name field. This observation draws my interest in looking deeply into the current state (*2024/08*) of submitted entities of the organization's CA, in terms of surveying the type and size/magnitude of errors present in the system.

It is paramount to fix errors in the system for obvious resasons. Specifically, identifying and rectifying the errors will not only support users or researchers to make accurate and precise reference searching for the material but also aid archivists in refininig their standards and guideline for entity entry practices. 

---
To identify potential erronenous/duplicate entities, we have first need to retrieve all entities from CA, calculate edit distance between words and then characterize the variation between similar words. This procedure is divided into into three parts, one per jupyter notebook, as follows:
   
* `01_entities_assessment`: This notebook extracts all individual and company entities, as of date (2024/08/10), from CollectiveAcess using [GraphQL API's search](https://manual.collectiveaccess.org/providence/developer/web_api/graphql/search.html). The list of entities are saved as a pickle file. 
* `02_similarity_measurement_entities_names`: This procedure takes in the list of individuals entities, preprocesses it, calculates pairwise edit distance across all individual entities and returns in the form of a numpy matrix. 
* `03_characteristic_of_similar_entities`: This final step examines entities that share words with no or one character difference and place them into categories e.g exact duplicate, 1 character difference with subsitution, etc. This table generate two columns text files for each of the categories. 

---

##### Prequistes on running the notebook

To run the Jupyter notebook, Python v3.0+, `Juptyer`, `pickle` and `editdistance` are required (e.g `pip install pickle`). See Conda & [Jupyter installation](https://jupyter.org/install) tutorial. 

You will need to create a login file named `login` with your username and password, separated by newline, in order to establish credential connection with CA.

#### Discovery

As of 2024/08/10, JP's CA had a record of 33,020 individuals, where 32,169 of the display names that contains more than one word. The current study excludes entities with one word name (saved for future separate analysis).

When considering the range of possible casual events that results in multiple submission entries of a single individual i.e the event of typographical or clerical entry (from the source material or during data entry), the limited or full access to the dated complete person's name, abbreviation or aliases, words of a person's name can be misspelled, omitted or completely distinct. 

Given the large number of entities in CA along with a wide possible range of errors, the current study will honed onto simple case scenario, specifically on entitiy pairs that are only edit distance of 0 or 1 unit apart between 'matching' words but no penalty imposed upon extra or missing words. Prior to calculate edit distance, I have also removed any non-alpha numeric character from entity names. 

 Here's a summary table that reports the number of unique entity pairing for each category:

| Categories     | #    | Subcategories  | #    | Subategories | #    |
|----------------|------|-------------|------|------------|------|
| No extra word  | 1522 | Duplicate   |  414 | exact      | 250  |
|                |      |             |      | special    | 164  |
|                |      | 1 char typo | 1098 | ins/del    | 552  |
|                |      |             |      | sub        | 546  |
| 1 extra word   | 800  | no typo     |  495 |            |      |
|                |      | 1 char typo |  305 |            |      |
| 2+ extra words |  295 | no typo     |  213 |            |      |
|                |      | 1 char typo |   82 |            |      |

Two subcategories can be found under the `duplicate` category. The `special duplicate` case refers to perfect matches of all alpha-numeric characters but not for non-alphanumeric characters for each word between entities. 

The list of entity pairs for each purported erronenous categories is a tab-separated txt file. Here are some patterns / examples found in each output files.
* exact_dup.txt
* special_dup.txt
    * extra quotes or characters
    ```
    Leslie "Bubba" Gaines   Leslie Bubba Gaines
    Claire O'Halloran       Claire O’Halloran
    John Evans      John Evans.
    Judith E James  Judith E. James
    Kendra Wilsher  `Kendra Wilsher
    Sir Brock Warren        Sir. Brock Warren       
    Young-Sil Kim   Youngsil Kim    
    ``` 
    * cases of names  w/ extra characters but in different rearrangements
    ```
    Baer, Nancy Van Norman      Van Norman Baer, Nancy
    Louis M. Starr      Starr, Louis M.
    ```
* extra_word_0_char_typo_1.txt
    * cases of substitution
    ```
    Carly Greene    Greene, Carla
    Harry Warren    Warren, Larry
    James Truitee   James Truitte
    Sahome Tachibana        Sahomi Tachibana
    ```
    * cases of insertion / deletion
    ```
    Norton Owen     Noton Owen
    Ellen Jacobs    Jacob, Ellen
    Erick Hawkins   Erik Hawkins
    Li-Chuan Liao   Li-Chuang Liao
    Jule Styne      Julie Styne
    ```
* extra_word_1_char_typo_0.txt
    * organization entity mixed in with individual entity
    ```
    Margo Jones     Margo Jones Architects
    Nel Shelby      Nel Shelby Productions
    Wally Cardona   Wally Cardona Quartet
    Martha Swope    Martha Swope Associates
    ```
    * Additional title, middlename, alias, etc
    ```
    George Skibine                  George Skibine Jr
    Ferd "Jelly Roll" Morton        Jelly Roll Morton
    Keith A. Thompson               Keith Thompson
    Christopher K. Morgan           Christopher Morgan
    Jose Greco                      Jose Greco II
    Gus Solomons                    Gus Solomons Jr.
    Wanderlino Martins Neves        Wanderlino Martins Neves "Sorriso"
    ```
    * Weird export artifacts & special keywords (amp, et, and, y)
    ```
    Antonia Gabarri                         music: Antonia Gabarri
    Rosane Chamecki &amp; Andrea Lerner     Rosane Chamecki; Andrea Lerner
    Donna McKechnie vocalist                Donna McKechnie
    Jacob's Pillow                          Jacob's Pillow Archives
    R&amp;T Khemese S.                      R&amp;T Khemese
    ```
* extra_word_1_char_typo_1.txt
    * intermix of true positve & false positive reported
    ```
    Amith A. Chandrashaker  Amith Chandashaker
    David Hays      David La Hay
    Brian Lawson    Brian T. Lawton
    R. Kelly        Rocky Kelly Jr.
    Leslie E. Williams      S. Williams
    Jah’meek D. Williams    S. Williams
    Belanger, Pamela J.     Pamela Z
    ```
* extra_word_gt1_char_typo_0.txt
    * another case of company entities among individual entity
    ```
    Dance Horizons  Dance Horizons Spotlight Series
    Judy Dworin     Judy Dworin Performance Ensemble
    Kathryn Posin   The Kathryn Posin Dance Co.
    aura Dean       Laura Dean and Dancers and Musicians
    ```
    * 2 or more entities labeled as one
    ```
    Stephan Koplowitz       Stephan Koplowitz; Jack Freudenheim
    Astor Piazzolla         Astor Piazzolla &amp; Jerzy Peterburshsky
    Xenia Rocco             Xenia Rocco and Penelope Wendtlandt.
    Emily Martin            Lee, Martin and Emily
    DD Dorvillier           Dana Salisbury; Ariane Anthony; Christopher Caines; DD Dorvillier
    Jean Leon Destine       Jeanne Ramon; Jean Leon Destine
    ```
    * alias & extra long appended names
    ```
    DJ Evil Tracy   Tracy Thomas aka "DJ Evil Tracy"
    Gilbert Small   Gilbert T Small II
    Andre Gaines    Andre Strongbearheart Gaines Jr
    ```

* extra_word_gt1_char_typo_1.txt
    * Hodgepode of true and false erroneous cases
    
    ```
    Amy Marshall                    Copeland, Roger &amp; Cohen, Marshall
    Nancy Bae                       Van Norman Baer, Nancy
    Sun Yong Kim and Taik Won Cho   Yang Sun
    Jose Alfredo Jimenez Sandoval   Jose Zimenez
    Sun Yong Kim and Taik Won Cho   Yang Sun
    An Evening of Cambodian Dance and Music Music and Dance of Cambodia
    Sara Hinding, Alex Johnson      Sarah Johnson
    Lester Bowie    Lester Bowie's Brass Fantasy
    ```

For now, my recommendation that to resolve any pairs that has at least 1 typo requires extra care, research, and validation (verify through looking at associated programs and online searches). 



#### Future Tasks/TODO
Need to write automate script to merge entities that are truly duplicate and to validate spelling typo by text scraping programs note associated with the entities. 
 