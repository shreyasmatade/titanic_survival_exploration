./__pycache__/                                                                                      0000755 0000000 0000000 00000000000 13417507300 012121  5                                                                                                    ustar   root                            root                                                                                                                                                                                                                   ./__pycache__/visuals.cpython-36.pyc                                                                0000644 0000000 0000000 00000006735 13312321400 016235  0                                                                                                    ustar   root                            root                                                                                                                                                                                                                   3
>�VZ�  �               @   sd   d dl Z e jdedd� d dlmZ e� jdd� d dlZd dlZ	d dl
jZdd� Zg fd	d
�ZdS )�    N�ignore�
matplotlib)�category�module)�get_ipython�inlinec          	   C   s�   |j d�\}}}yt|�}W n   |jd�}Y nX |dkrJ| | |k}nv|dkr`| | |k }n`|dkrv| | |k}nJ|dkr�| | |k}n4|dkr�| | |k}n|dkr�| | |k}ntd	��| | jd
d�} | S )aS  
    Remove elements that do not match the condition provided.
    Takes a data list as input and returns a filtered list.
    Conditions should be a list of strings of the following format:
      '<field> <op> <value>'
    where the following operations are valid: >, <, >=, <=, ==, !=
    
    Example: ["Sex == 'male'", 'Age < 18']
    � z'"�>�<z>=z<=z==z!=z?Invalid comparison operator. Only >, <, >=, <=, ==, != allowed.T)�drop)�split�float�strip�	Exception�reset_index)�data�	condition�field�op�value�matches� r   �/home/workspace/visuals.py�filter_data   s(    r   c             C   s�  || j jkrtdj|�� dS |dks6|dks6|dkrHtdj|�� dS tj| |j� gdd�}x|D ]}t||�}qdW ||d	g }tj	d0d� |dks�|dk�r�|t
j|| �  }|| j� }|| j� }|| }|dkr�t
jd|d j� d d�}	|dk�rt
jd|d j� d d�}	||d	 dk | jdd�}
||d	 dk | jdd�}tj|
|	dddd� tj||	ddd	d� tjd|	j� � tjdd� �nt|dk�r�t
jdd�}|dk�s�|dk�r�t
jdt
j| | �d �}|dk�r�d d!d"g}|d#k�rd$d%g}tjt
jt|��|d	d&fd'�}x^t|�D ]R\}}|t||d	 dk|| |k@  �t||d	 dk|| |k@  �g|j|< �q,W d(}x�t
jt|��D ]t}tj|| |j| d& |d)d*�}tj||j| d	 |d+d*�}tjt
jt|��|� tj|d |d fd1dd� �q�W tj|� tjd,� tjd-| � tj�  ttj|| ���r�|tj|| � d	 }td.j|t|�t|dk�t|dk��� d/S )2z�
    Print out selected statistics regarding survival, given a feature of
    interest and any number of filters (including no filters)
    zI'{}' is not a feature of the Titanic data. Did you spell something wrong?FZCabinZPassengerIdZTicketzH'{}' has too many unique categories to display! Try a different feature.�   )�axis�Survived�   �   )�figsizeZAgeZFarer   �   �
   T)r   g333333�?�red�Did not survive)�bins�alpha�color�label�greeng�������?)Z
framealphaZPclass�   ZParchZSibSpZEmbarked�C�Q�SZSexZmaleZfemaleZ	NSurvived)�index�columnsg�������?�r)�widthr&   �gzNumber of Passengersz/Passenger Survival Statistics With '%s' FeaturezIPassengers with missing '{}' values: {} ({} survived, {} did not survive)N)r   r   )r#   r   )r.   �values�print�format�pd�concat�to_framer   �plt�figure�np�isnan�min�max�aranger   �hist�xlim�legend�	DataFrame�len�	enumerate�loc�bar�xticksZxlabelZylabel�title�show�sum�isnull)r   Zoutcomes�key�filtersZall_datar   �	min_valueZ	max_valueZvalue_ranger$   Znonsurv_valsZ	surv_valsr2   �frame�ir   �	bar_widthZnonsurv_barZsurv_barZnan_outcomesr   r   r   �survival_stats7   sn    









. "

rR   )�warnings�filterwarnings�UserWarning�IPythonr   �run_line_magic�numpyr:   �pandasr5   �matplotlib.pyplot�pyplotr8   r   rR   r   r   r   r   �<module>   s   
'                                   ./README.md                                                                                         0000644 0000000 0000000 00000004450 13225223106 011167  0                                                                                                    ustar   root                            root                                                                                                                                                                                                                   # Machine Learning Engineer Nanodegree
## Introduction and Foundations
## Project: Titanic Survival Exploration

### Install

This project requires **Python 2.7** and the following Python libraries installed:

- [NumPy](http://www.numpy.org/)
- [Pandas](http://pandas.pydata.org)
- [matplotlib](http://matplotlib.org/)
- [scikit-learn](http://scikit-learn.org/stable/)

You will also need to have software installed to run and execute a [Jupyter Notebook](http://ipython.org/notebook.html)

If you do not have Python installed yet, it is highly recommended that you install the [Anaconda](http://continuum.io/downloads) distribution of Python, which already has the above packages and more included. Make sure that you select the Python 2.7 installer and not the Python 3.x installer.

### Code

Template code is provided in the notebook `titanic_survival_exploration.ipynb` notebook file. Additional supporting code can be found in `visuals.py`. While some code has already been implemented to get you started, you will need to implement additional functionality when requested to successfully complete the project. Note that the code included in `visuals.py` is meant to be used out-of-the-box and not intended for students to manipulate. If you are interested in how the visualizations are created in the notebook, please feel free to explore this Python file.

### Run

In a terminal or command window, navigate to the top-level project directory `titanic_survival_exploration/` (that contains this README) and run one of the following commands:

```bash
jupyter notebook titanic_survival_exploration.ipynb
```
or
```bash
ipython notebook titanic_survival_exploration.ipynb
```

This will open the Jupyter Notebook software and project file in your web browser.

### Data

The dataset used in this project is included as `titanic_data.csv`. This dataset is provided by Udacity and contains the following attributes:

**Features**
- `pclass` : Passenger Class (1 = 1st; 2 = 2nd; 3 = 3rd)
- `name` : Name
- `sex` : Sex
- `age` : Age
- `sibsp` : Number of Siblings/Spouses Aboard
- `parch` : Number of Parents/Children Aboard
- `ticket` : Ticket Number
- `fare` : Passenger Fare
- `cabin` : Cabin
- `embarked` : Port of Embarkation (C = Cherbourg; Q = Queenstown; S = Southampton)

**Target Variable**
- `survival` : Survival (0 = No; 1 = Yes)                                                                                                                                                                                                                        ./titanic_data.csv                                                                                  0000644 0000000 0000000 00000167412 13225223105 013060  0                                                                                                    ustar   root                            root                                                                                                                                                                                                                   PassengerId,Survived,Pclass,Name,Sex,Age,SibSp,Parch,Ticket,Fare,Cabin,Embarked
1,0,3,"Braund, Mr. Owen Harris",male,22,1,0,A/5 21171,7.25,,S
2,1,1,"Cumings, Mrs. John Bradley (Florence Briggs Thayer)",female,38,1,0,PC 17599,71.2833,C85,C
3,1,3,"Heikkinen, Miss. Laina",female,26,0,0,STON/O2. 3101282,7.925,,S
4,1,1,"Futrelle, Mrs. Jacques Heath (Lily May Peel)",female,35,1,0,113803,53.1,C123,S
5,0,3,"Allen, Mr. William Henry",male,35,0,0,373450,8.05,,S
6,0,3,"Moran, Mr. James",male,,0,0,330877,8.4583,,Q
7,0,1,"McCarthy, Mr. Timothy J",male,54,0,0,17463,51.8625,E46,S
8,0,3,"Palsson, Master. Gosta Leonard",male,2,3,1,349909,21.075,,S
9,1,3,"Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)",female,27,0,2,347742,11.1333,,S
10,1,2,"Nasser, Mrs. Nicholas (Adele Achem)",female,14,1,0,237736,30.0708,,C
11,1,3,"Sandstrom, Miss. Marguerite Rut",female,4,1,1,PP 9549,16.7,G6,S
12,1,1,"Bonnell, Miss. Elizabeth",female,58,0,0,113783,26.55,C103,S
13,0,3,"Saundercock, Mr. William Henry",male,20,0,0,A/5. 2151,8.05,,S
14,0,3,"Andersson, Mr. Anders Johan",male,39,1,5,347082,31.275,,S
15,0,3,"Vestrom, Miss. Hulda Amanda Adolfina",female,14,0,0,350406,7.8542,,S
16,1,2,"Hewlett, Mrs. (Mary D Kingcome) ",female,55,0,0,248706,16,,S
17,0,3,"Rice, Master. Eugene",male,2,4,1,382652,29.125,,Q
18,1,2,"Williams, Mr. Charles Eugene",male,,0,0,244373,13,,S
19,0,3,"Vander Planke, Mrs. Julius (Emelia Maria Vandemoortele)",female,31,1,0,345763,18,,S
20,1,3,"Masselmani, Mrs. Fatima",female,,0,0,2649,7.225,,C
21,0,2,"Fynney, Mr. Joseph J",male,35,0,0,239865,26,,S
22,1,2,"Beesley, Mr. Lawrence",male,34,0,0,248698,13,D56,S
23,1,3,"McGowan, Miss. Anna ""Annie""",female,15,0,0,330923,8.0292,,Q
24,1,1,"Sloper, Mr. William Thompson",male,28,0,0,113788,35.5,A6,S
25,0,3,"Palsson, Miss. Torborg Danira",female,8,3,1,349909,21.075,,S
26,1,3,"Asplund, Mrs. Carl Oscar (Selma Augusta Emilia Johansson)",female,38,1,5,347077,31.3875,,S
27,0,3,"Emir, Mr. Farred Chehab",male,,0,0,2631,7.225,,C
28,0,1,"Fortune, Mr. Charles Alexander",male,19,3,2,19950,263,C23 C25 C27,S
29,1,3,"O'Dwyer, Miss. Ellen ""Nellie""",female,,0,0,330959,7.8792,,Q
30,0,3,"Todoroff, Mr. Lalio",male,,0,0,349216,7.8958,,S
31,0,1,"Uruchurtu, Don. Manuel E",male,40,0,0,PC 17601,27.7208,,C
32,1,1,"Spencer, Mrs. William Augustus (Marie Eugenie)",female,,1,0,PC 17569,146.5208,B78,C
33,1,3,"Glynn, Miss. Mary Agatha",female,,0,0,335677,7.75,,Q
34,0,2,"Wheadon, Mr. Edward H",male,66,0,0,C.A. 24579,10.5,,S
35,0,1,"Meyer, Mr. Edgar Joseph",male,28,1,0,PC 17604,82.1708,,C
36,0,1,"Holverson, Mr. Alexander Oskar",male,42,1,0,113789,52,,S
37,1,3,"Mamee, Mr. Hanna",male,,0,0,2677,7.2292,,C
38,0,3,"Cann, Mr. Ernest Charles",male,21,0,0,A./5. 2152,8.05,,S
39,0,3,"Vander Planke, Miss. Augusta Maria",female,18,2,0,345764,18,,S
40,1,3,"Nicola-Yarred, Miss. Jamila",female,14,1,0,2651,11.2417,,C
41,0,3,"Ahlin, Mrs. Johan (Johanna Persdotter Larsson)",female,40,1,0,7546,9.475,,S
42,0,2,"Turpin, Mrs. William John Robert (Dorothy Ann Wonnacott)",female,27,1,0,11668,21,,S
43,0,3,"Kraeff, Mr. Theodor",male,,0,0,349253,7.8958,,C
44,1,2,"Laroche, Miss. Simonne Marie Anne Andree",female,3,1,2,SC/Paris 2123,41.5792,,C
45,1,3,"Devaney, Miss. Margaret Delia",female,19,0,0,330958,7.8792,,Q
46,0,3,"Rogers, Mr. William John",male,,0,0,S.C./A.4. 23567,8.05,,S
47,0,3,"Lennon, Mr. Denis",male,,1,0,370371,15.5,,Q
48,1,3,"O'Driscoll, Miss. Bridget",female,,0,0,14311,7.75,,Q
49,0,3,"Samaan, Mr. Youssef",male,,2,0,2662,21.6792,,C
50,0,3,"Arnold-Franchi, Mrs. Josef (Josefine Franchi)",female,18,1,0,349237,17.8,,S
51,0,3,"Panula, Master. Juha Niilo",male,7,4,1,3101295,39.6875,,S
52,0,3,"Nosworthy, Mr. Richard Cater",male,21,0,0,A/4. 39886,7.8,,S
53,1,1,"Harper, Mrs. Henry Sleeper (Myna Haxtun)",female,49,1,0,PC 17572,76.7292,D33,C
54,1,2,"Faunthorpe, Mrs. Lizzie (Elizabeth Anne Wilkinson)",female,29,1,0,2926,26,,S
55,0,1,"Ostby, Mr. Engelhart Cornelius",male,65,0,1,113509,61.9792,B30,C
56,1,1,"Woolner, Mr. Hugh",male,,0,0,19947,35.5,C52,S
57,1,2,"Rugg, Miss. Emily",female,21,0,0,C.A. 31026,10.5,,S
58,0,3,"Novel, Mr. Mansouer",male,28.5,0,0,2697,7.2292,,C
59,1,2,"West, Miss. Constance Mirium",female,5,1,2,C.A. 34651,27.75,,S
60,0,3,"Goodwin, Master. William Frederick",male,11,5,2,CA 2144,46.9,,S
61,0,3,"Sirayanian, Mr. Orsen",male,22,0,0,2669,7.2292,,C
62,1,1,"Icard, Miss. Amelie",female,38,0,0,113572,80,B28,
63,0,1,"Harris, Mr. Henry Birkhardt",male,45,1,0,36973,83.475,C83,S
64,0,3,"Skoog, Master. Harald",male,4,3,2,347088,27.9,,S
65,0,1,"Stewart, Mr. Albert A",male,,0,0,PC 17605,27.7208,,C
66,1,3,"Moubarek, Master. Gerios",male,,1,1,2661,15.2458,,C
67,1,2,"Nye, Mrs. (Elizabeth Ramell)",female,29,0,0,C.A. 29395,10.5,F33,S
68,0,3,"Crease, Mr. Ernest James",male,19,0,0,S.P. 3464,8.1583,,S
69,1,3,"Andersson, Miss. Erna Alexandra",female,17,4,2,3101281,7.925,,S
70,0,3,"Kink, Mr. Vincenz",male,26,2,0,315151,8.6625,,S
71,0,2,"Jenkin, Mr. Stephen Curnow",male,32,0,0,C.A. 33111,10.5,,S
72,0,3,"Goodwin, Miss. Lillian Amy",female,16,5,2,CA 2144,46.9,,S
73,0,2,"Hood, Mr. Ambrose Jr",male,21,0,0,S.O.C. 14879,73.5,,S
74,0,3,"Chronopoulos, Mr. Apostolos",male,26,1,0,2680,14.4542,,C
75,1,3,"Bing, Mr. Lee",male,32,0,0,1601,56.4958,,S
76,0,3,"Moen, Mr. Sigurd Hansen",male,25,0,0,348123,7.65,F G73,S
77,0,3,"Staneff, Mr. Ivan",male,,0,0,349208,7.8958,,S
78,0,3,"Moutal, Mr. Rahamin Haim",male,,0,0,374746,8.05,,S
79,1,2,"Caldwell, Master. Alden Gates",male,0.83,0,2,248738,29,,S
80,1,3,"Dowdell, Miss. Elizabeth",female,30,0,0,364516,12.475,,S
81,0,3,"Waelens, Mr. Achille",male,22,0,0,345767,9,,S
82,1,3,"Sheerlinck, Mr. Jan Baptist",male,29,0,0,345779,9.5,,S
83,1,3,"McDermott, Miss. Brigdet Delia",female,,0,0,330932,7.7875,,Q
84,0,1,"Carrau, Mr. Francisco M",male,28,0,0,113059,47.1,,S
85,1,2,"Ilett, Miss. Bertha",female,17,0,0,SO/C 14885,10.5,,S
86,1,3,"Backstrom, Mrs. Karl Alfred (Maria Mathilda Gustafsson)",female,33,3,0,3101278,15.85,,S
87,0,3,"Ford, Mr. William Neal",male,16,1,3,W./C. 6608,34.375,,S
88,0,3,"Slocovski, Mr. Selman Francis",male,,0,0,SOTON/OQ 392086,8.05,,S
89,1,1,"Fortune, Miss. Mabel Helen",female,23,3,2,19950,263,C23 C25 C27,S
90,0,3,"Celotti, Mr. Francesco",male,24,0,0,343275,8.05,,S
91,0,3,"Christmann, Mr. Emil",male,29,0,0,343276,8.05,,S
92,0,3,"Andreasson, Mr. Paul Edvin",male,20,0,0,347466,7.8542,,S
93,0,1,"Chaffee, Mr. Herbert Fuller",male,46,1,0,W.E.P. 5734,61.175,E31,S
94,0,3,"Dean, Mr. Bertram Frank",male,26,1,2,C.A. 2315,20.575,,S
95,0,3,"Coxon, Mr. Daniel",male,59,0,0,364500,7.25,,S
96,0,3,"Shorney, Mr. Charles Joseph",male,,0,0,374910,8.05,,S
97,0,1,"Goldschmidt, Mr. George B",male,71,0,0,PC 17754,34.6542,A5,C
98,1,1,"Greenfield, Mr. William Bertram",male,23,0,1,PC 17759,63.3583,D10 D12,C
99,1,2,"Doling, Mrs. John T (Ada Julia Bone)",female,34,0,1,231919,23,,S
100,0,2,"Kantor, Mr. Sinai",male,34,1,0,244367,26,,S
101,0,3,"Petranec, Miss. Matilda",female,28,0,0,349245,7.8958,,S
102,0,3,"Petroff, Mr. Pastcho (""Pentcho"")",male,,0,0,349215,7.8958,,S
103,0,1,"White, Mr. Richard Frasar",male,21,0,1,35281,77.2875,D26,S
104,0,3,"Johansson, Mr. Gustaf Joel",male,33,0,0,7540,8.6542,,S
105,0,3,"Gustafsson, Mr. Anders Vilhelm",male,37,2,0,3101276,7.925,,S
106,0,3,"Mionoff, Mr. Stoytcho",male,28,0,0,349207,7.8958,,S
107,1,3,"Salkjelsvik, Miss. Anna Kristine",female,21,0,0,343120,7.65,,S
108,1,3,"Moss, Mr. Albert Johan",male,,0,0,312991,7.775,,S
109,0,3,"Rekic, Mr. Tido",male,38,0,0,349249,7.8958,,S
110,1,3,"Moran, Miss. Bertha",female,,1,0,371110,24.15,,Q
111,0,1,"Porter, Mr. Walter Chamberlain",male,47,0,0,110465,52,C110,S
112,0,3,"Zabour, Miss. Hileni",female,14.5,1,0,2665,14.4542,,C
113,0,3,"Barton, Mr. David John",male,22,0,0,324669,8.05,,S
114,0,3,"Jussila, Miss. Katriina",female,20,1,0,4136,9.825,,S
115,0,3,"Attalah, Miss. Malake",female,17,0,0,2627,14.4583,,C
116,0,3,"Pekoniemi, Mr. Edvard",male,21,0,0,STON/O 2. 3101294,7.925,,S
117,0,3,"Connors, Mr. Patrick",male,70.5,0,0,370369,7.75,,Q
118,0,2,"Turpin, Mr. William John Robert",male,29,1,0,11668,21,,S
119,0,1,"Baxter, Mr. Quigg Edmond",male,24,0,1,PC 17558,247.5208,B58 B60,C
120,0,3,"Andersson, Miss. Ellis Anna Maria",female,2,4,2,347082,31.275,,S
121,0,2,"Hickman, Mr. Stanley George",male,21,2,0,S.O.C. 14879,73.5,,S
122,0,3,"Moore, Mr. Leonard Charles",male,,0,0,A4. 54510,8.05,,S
123,0,2,"Nasser, Mr. Nicholas",male,32.5,1,0,237736,30.0708,,C
124,1,2,"Webber, Miss. Susan",female,32.5,0,0,27267,13,E101,S
125,0,1,"White, Mr. Percival Wayland",male,54,0,1,35281,77.2875,D26,S
126,1,3,"Nicola-Yarred, Master. Elias",male,12,1,0,2651,11.2417,,C
127,0,3,"McMahon, Mr. Martin",male,,0,0,370372,7.75,,Q
128,1,3,"Madsen, Mr. Fridtjof Arne",male,24,0,0,C 17369,7.1417,,S
129,1,3,"Peter, Miss. Anna",female,,1,1,2668,22.3583,F E69,C
130,0,3,"Ekstrom, Mr. Johan",male,45,0,0,347061,6.975,,S
131,0,3,"Drazenoic, Mr. Jozef",male,33,0,0,349241,7.8958,,C
132,0,3,"Coelho, Mr. Domingos Fernandeo",male,20,0,0,SOTON/O.Q. 3101307,7.05,,S
133,0,3,"Robins, Mrs. Alexander A (Grace Charity Laury)",female,47,1,0,A/5. 3337,14.5,,S
134,1,2,"Weisz, Mrs. Leopold (Mathilde Francoise Pede)",female,29,1,0,228414,26,,S
135,0,2,"Sobey, Mr. Samuel James Hayden",male,25,0,0,C.A. 29178,13,,S
136,0,2,"Richard, Mr. Emile",male,23,0,0,SC/PARIS 2133,15.0458,,C
137,1,1,"Newsom, Miss. Helen Monypeny",female,19,0,2,11752,26.2833,D47,S
138,0,1,"Futrelle, Mr. Jacques Heath",male,37,1,0,113803,53.1,C123,S
139,0,3,"Osen, Mr. Olaf Elon",male,16,0,0,7534,9.2167,,S
140,0,1,"Giglio, Mr. Victor",male,24,0,0,PC 17593,79.2,B86,C
141,0,3,"Boulos, Mrs. Joseph (Sultana)",female,,0,2,2678,15.2458,,C
142,1,3,"Nysten, Miss. Anna Sofia",female,22,0,0,347081,7.75,,S
143,1,3,"Hakkarainen, Mrs. Pekka Pietari (Elin Matilda Dolck)",female,24,1,0,STON/O2. 3101279,15.85,,S
144,0,3,"Burke, Mr. Jeremiah",male,19,0,0,365222,6.75,,Q
145,0,2,"Andrew, Mr. Edgardo Samuel",male,18,0,0,231945,11.5,,S
146,0,2,"Nicholls, Mr. Joseph Charles",male,19,1,1,C.A. 33112,36.75,,S
147,1,3,"Andersson, Mr. August Edvard (""Wennerstrom"")",male,27,0,0,350043,7.7958,,S
148,0,3,"Ford, Miss. Robina Maggie ""Ruby""",female,9,2,2,W./C. 6608,34.375,,S
149,0,2,"Navratil, Mr. Michel (""Louis M Hoffman"")",male,36.5,0,2,230080,26,F2,S
150,0,2,"Byles, Rev. Thomas Roussel Davids",male,42,0,0,244310,13,,S
151,0,2,"Bateman, Rev. Robert James",male,51,0,0,S.O.P. 1166,12.525,,S
152,1,1,"Pears, Mrs. Thomas (Edith Wearne)",female,22,1,0,113776,66.6,C2,S
153,0,3,"Meo, Mr. Alfonzo",male,55.5,0,0,A.5. 11206,8.05,,S
154,0,3,"van Billiard, Mr. Austin Blyler",male,40.5,0,2,A/5. 851,14.5,,S
155,0,3,"Olsen, Mr. Ole Martin",male,,0,0,Fa 265302,7.3125,,S
156,0,1,"Williams, Mr. Charles Duane",male,51,0,1,PC 17597,61.3792,,C
157,1,3,"Gilnagh, Miss. Katherine ""Katie""",female,16,0,0,35851,7.7333,,Q
158,0,3,"Corn, Mr. Harry",male,30,0,0,SOTON/OQ 392090,8.05,,S
159,0,3,"Smiljanic, Mr. Mile",male,,0,0,315037,8.6625,,S
160,0,3,"Sage, Master. Thomas Henry",male,,8,2,CA. 2343,69.55,,S
161,0,3,"Cribb, Mr. John Hatfield",male,44,0,1,371362,16.1,,S
162,1,2,"Watt, Mrs. James (Elizabeth ""Bessie"" Inglis Milne)",female,40,0,0,C.A. 33595,15.75,,S
163,0,3,"Bengtsson, Mr. John Viktor",male,26,0,0,347068,7.775,,S
164,0,3,"Calic, Mr. Jovo",male,17,0,0,315093,8.6625,,S
165,0,3,"Panula, Master. Eino Viljami",male,1,4,1,3101295,39.6875,,S
166,1,3,"Goldsmith, Master. Frank John William ""Frankie""",male,9,0,2,363291,20.525,,S
167,1,1,"Chibnall, Mrs. (Edith Martha Bowerman)",female,,0,1,113505,55,E33,S
168,0,3,"Skoog, Mrs. William (Anna Bernhardina Karlsson)",female,45,1,4,347088,27.9,,S
169,0,1,"Baumann, Mr. John D",male,,0,0,PC 17318,25.925,,S
170,0,3,"Ling, Mr. Lee",male,28,0,0,1601,56.4958,,S
171,0,1,"Van der hoef, Mr. Wyckoff",male,61,0,0,111240,33.5,B19,S
172,0,3,"Rice, Master. Arthur",male,4,4,1,382652,29.125,,Q
173,1,3,"Johnson, Miss. Eleanor Ileen",female,1,1,1,347742,11.1333,,S
174,0,3,"Sivola, Mr. Antti Wilhelm",male,21,0,0,STON/O 2. 3101280,7.925,,S
175,0,1,"Smith, Mr. James Clinch",male,56,0,0,17764,30.6958,A7,C
176,0,3,"Klasen, Mr. Klas Albin",male,18,1,1,350404,7.8542,,S
177,0,3,"Lefebre, Master. Henry Forbes",male,,3,1,4133,25.4667,,S
178,0,1,"Isham, Miss. Ann Elizabeth",female,50,0,0,PC 17595,28.7125,C49,C
179,0,2,"Hale, Mr. Reginald",male,30,0,0,250653,13,,S
180,0,3,"Leonard, Mr. Lionel",male,36,0,0,LINE,0,,S
181,0,3,"Sage, Miss. Constance Gladys",female,,8,2,CA. 2343,69.55,,S
182,0,2,"Pernot, Mr. Rene",male,,0,0,SC/PARIS 2131,15.05,,C
183,0,3,"Asplund, Master. Clarence Gustaf Hugo",male,9,4,2,347077,31.3875,,S
184,1,2,"Becker, Master. Richard F",male,1,2,1,230136,39,F4,S
185,1,3,"Kink-Heilmann, Miss. Luise Gretchen",female,4,0,2,315153,22.025,,S
186,0,1,"Rood, Mr. Hugh Roscoe",male,,0,0,113767,50,A32,S
187,1,3,"O'Brien, Mrs. Thomas (Johanna ""Hannah"" Godfrey)",female,,1,0,370365,15.5,,Q
188,1,1,"Romaine, Mr. Charles Hallace (""Mr C Rolmane"")",male,45,0,0,111428,26.55,,S
189,0,3,"Bourke, Mr. John",male,40,1,1,364849,15.5,,Q
190,0,3,"Turcin, Mr. Stjepan",male,36,0,0,349247,7.8958,,S
191,1,2,"Pinsky, Mrs. (Rosa)",female,32,0,0,234604,13,,S
192,0,2,"Carbines, Mr. William",male,19,0,0,28424,13,,S
193,1,3,"Andersen-Jensen, Miss. Carla Christine Nielsine",female,19,1,0,350046,7.8542,,S
194,1,2,"Navratil, Master. Michel M",male,3,1,1,230080,26,F2,S
195,1,1,"Brown, Mrs. James Joseph (Margaret Tobin)",female,44,0,0,PC 17610,27.7208,B4,C
196,1,1,"Lurette, Miss. Elise",female,58,0,0,PC 17569,146.5208,B80,C
197,0,3,"Mernagh, Mr. Robert",male,,0,0,368703,7.75,,Q
198,0,3,"Olsen, Mr. Karl Siegwart Andreas",male,42,0,1,4579,8.4042,,S
199,1,3,"Madigan, Miss. Margaret ""Maggie""",female,,0,0,370370,7.75,,Q
200,0,2,"Yrois, Miss. Henriette (""Mrs Harbeck"")",female,24,0,0,248747,13,,S
201,0,3,"Vande Walle, Mr. Nestor Cyriel",male,28,0,0,345770,9.5,,S
202,0,3,"Sage, Mr. Frederick",male,,8,2,CA. 2343,69.55,,S
203,0,3,"Johanson, Mr. Jakob Alfred",male,34,0,0,3101264,6.4958,,S
204,0,3,"Youseff, Mr. Gerious",male,45.5,0,0,2628,7.225,,C
205,1,3,"Cohen, Mr. Gurshon ""Gus""",male,18,0,0,A/5 3540,8.05,,S
206,0,3,"Strom, Miss. Telma Matilda",female,2,0,1,347054,10.4625,G6,S
207,0,3,"Backstrom, Mr. Karl Alfred",male,32,1,0,3101278,15.85,,S
208,1,3,"Albimona, Mr. Nassef Cassem",male,26,0,0,2699,18.7875,,C
209,1,3,"Carr, Miss. Helen ""Ellen""",female,16,0,0,367231,7.75,,Q
210,1,1,"Blank, Mr. Henry",male,40,0,0,112277,31,A31,C
211,0,3,"Ali, Mr. Ahmed",male,24,0,0,SOTON/O.Q. 3101311,7.05,,S
212,1,2,"Cameron, Miss. Clear Annie",female,35,0,0,F.C.C. 13528,21,,S
213,0,3,"Perkin, Mr. John Henry",male,22,0,0,A/5 21174,7.25,,S
214,0,2,"Givard, Mr. Hans Kristensen",male,30,0,0,250646,13,,S
215,0,3,"Kiernan, Mr. Philip",male,,1,0,367229,7.75,,Q
216,1,1,"Newell, Miss. Madeleine",female,31,1,0,35273,113.275,D36,C
217,1,3,"Honkanen, Miss. Eliina",female,27,0,0,STON/O2. 3101283,7.925,,S
218,0,2,"Jacobsohn, Mr. Sidney Samuel",male,42,1,0,243847,27,,S
219,1,1,"Bazzani, Miss. Albina",female,32,0,0,11813,76.2917,D15,C
220,0,2,"Harris, Mr. Walter",male,30,0,0,W/C 14208,10.5,,S
221,1,3,"Sunderland, Mr. Victor Francis",male,16,0,0,SOTON/OQ 392089,8.05,,S
222,0,2,"Bracken, Mr. James H",male,27,0,0,220367,13,,S
223,0,3,"Green, Mr. George Henry",male,51,0,0,21440,8.05,,S
224,0,3,"Nenkoff, Mr. Christo",male,,0,0,349234,7.8958,,S
225,1,1,"Hoyt, Mr. Frederick Maxfield",male,38,1,0,19943,90,C93,S
226,0,3,"Berglund, Mr. Karl Ivar Sven",male,22,0,0,PP 4348,9.35,,S
227,1,2,"Mellors, Mr. William John",male,19,0,0,SW/PP 751,10.5,,S
228,0,3,"Lovell, Mr. John Hall (""Henry"")",male,20.5,0,0,A/5 21173,7.25,,S
229,0,2,"Fahlstrom, Mr. Arne Jonas",male,18,0,0,236171,13,,S
230,0,3,"Lefebre, Miss. Mathilde",female,,3,1,4133,25.4667,,S
231,1,1,"Harris, Mrs. Henry Birkhardt (Irene Wallach)",female,35,1,0,36973,83.475,C83,S
232,0,3,"Larsson, Mr. Bengt Edvin",male,29,0,0,347067,7.775,,S
233,0,2,"Sjostedt, Mr. Ernst Adolf",male,59,0,0,237442,13.5,,S
234,1,3,"Asplund, Miss. Lillian Gertrud",female,5,4,2,347077,31.3875,,S
235,0,2,"Leyson, Mr. Robert William Norman",male,24,0,0,C.A. 29566,10.5,,S
236,0,3,"Harknett, Miss. Alice Phoebe",female,,0,0,W./C. 6609,7.55,,S
237,0,2,"Hold, Mr. Stephen",male,44,1,0,26707,26,,S
238,1,2,"Collyer, Miss. Marjorie ""Lottie""",female,8,0,2,C.A. 31921,26.25,,S
239,0,2,"Pengelly, Mr. Frederick William",male,19,0,0,28665,10.5,,S
240,0,2,"Hunt, Mr. George Henry",male,33,0,0,SCO/W 1585,12.275,,S
241,0,3,"Zabour, Miss. Thamine",female,,1,0,2665,14.4542,,C
242,1,3,"Murphy, Miss. Katherine ""Kate""",female,,1,0,367230,15.5,,Q
243,0,2,"Coleridge, Mr. Reginald Charles",male,29,0,0,W./C. 14263,10.5,,S
244,0,3,"Maenpaa, Mr. Matti Alexanteri",male,22,0,0,STON/O 2. 3101275,7.125,,S
245,0,3,"Attalah, Mr. Sleiman",male,30,0,0,2694,7.225,,C
246,0,1,"Minahan, Dr. William Edward",male,44,2,0,19928,90,C78,Q
247,0,3,"Lindahl, Miss. Agda Thorilda Viktoria",female,25,0,0,347071,7.775,,S
248,1,2,"Hamalainen, Mrs. William (Anna)",female,24,0,2,250649,14.5,,S
249,1,1,"Beckwith, Mr. Richard Leonard",male,37,1,1,11751,52.5542,D35,S
250,0,2,"Carter, Rev. Ernest Courtenay",male,54,1,0,244252,26,,S
251,0,3,"Reed, Mr. James George",male,,0,0,362316,7.25,,S
252,0,3,"Strom, Mrs. Wilhelm (Elna Matilda Persson)",female,29,1,1,347054,10.4625,G6,S
253,0,1,"Stead, Mr. William Thomas",male,62,0,0,113514,26.55,C87,S
254,0,3,"Lobb, Mr. William Arthur",male,30,1,0,A/5. 3336,16.1,,S
255,0,3,"Rosblom, Mrs. Viktor (Helena Wilhelmina)",female,41,0,2,370129,20.2125,,S
256,1,3,"Touma, Mrs. Darwis (Hanne Youssef Razi)",female,29,0,2,2650,15.2458,,C
257,1,1,"Thorne, Mrs. Gertrude Maybelle",female,,0,0,PC 17585,79.2,,C
258,1,1,"Cherry, Miss. Gladys",female,30,0,0,110152,86.5,B77,S
259,1,1,"Ward, Miss. Anna",female,35,0,0,PC 17755,512.3292,,C
260,1,2,"Parrish, Mrs. (Lutie Davis)",female,50,0,1,230433,26,,S
261,0,3,"Smith, Mr. Thomas",male,,0,0,384461,7.75,,Q
262,1,3,"Asplund, Master. Edvin Rojj Felix",male,3,4,2,347077,31.3875,,S
263,0,1,"Taussig, Mr. Emil",male,52,1,1,110413,79.65,E67,S
264,0,1,"Harrison, Mr. William",male,40,0,0,112059,0,B94,S
265,0,3,"Henry, Miss. Delia",female,,0,0,382649,7.75,,Q
266,0,2,"Reeves, Mr. David",male,36,0,0,C.A. 17248,10.5,,S
267,0,3,"Panula, Mr. Ernesti Arvid",male,16,4,1,3101295,39.6875,,S
268,1,3,"Persson, Mr. Ernst Ulrik",male,25,1,0,347083,7.775,,S
269,1,1,"Graham, Mrs. William Thompson (Edith Junkins)",female,58,0,1,PC 17582,153.4625,C125,S
270,1,1,"Bissette, Miss. Amelia",female,35,0,0,PC 17760,135.6333,C99,S
271,0,1,"Cairns, Mr. Alexander",male,,0,0,113798,31,,S
272,1,3,"Tornquist, Mr. William Henry",male,25,0,0,LINE,0,,S
273,1,2,"Mellinger, Mrs. (Elizabeth Anne Maidment)",female,41,0,1,250644,19.5,,S
274,0,1,"Natsch, Mr. Charles H",male,37,0,1,PC 17596,29.7,C118,C
275,1,3,"Healy, Miss. Hanora ""Nora""",female,,0,0,370375,7.75,,Q
276,1,1,"Andrews, Miss. Kornelia Theodosia",female,63,1,0,13502,77.9583,D7,S
277,0,3,"Lindblom, Miss. Augusta Charlotta",female,45,0,0,347073,7.75,,S
278,0,2,"Parkes, Mr. Francis ""Frank""",male,,0,0,239853,0,,S
279,0,3,"Rice, Master. Eric",male,7,4,1,382652,29.125,,Q
280,1,3,"Abbott, Mrs. Stanton (Rosa Hunt)",female,35,1,1,C.A. 2673,20.25,,S
281,0,3,"Duane, Mr. Frank",male,65,0,0,336439,7.75,,Q
282,0,3,"Olsson, Mr. Nils Johan Goransson",male,28,0,0,347464,7.8542,,S
283,0,3,"de Pelsmaeker, Mr. Alfons",male,16,0,0,345778,9.5,,S
284,1,3,"Dorking, Mr. Edward Arthur",male,19,0,0,A/5. 10482,8.05,,S
285,0,1,"Smith, Mr. Richard William",male,,0,0,113056,26,A19,S
286,0,3,"Stankovic, Mr. Ivan",male,33,0,0,349239,8.6625,,C
287,1,3,"de Mulder, Mr. Theodore",male,30,0,0,345774,9.5,,S
288,0,3,"Naidenoff, Mr. Penko",male,22,0,0,349206,7.8958,,S
289,1,2,"Hosono, Mr. Masabumi",male,42,0,0,237798,13,,S
290,1,3,"Connolly, Miss. Kate",female,22,0,0,370373,7.75,,Q
291,1,1,"Barber, Miss. Ellen ""Nellie""",female,26,0,0,19877,78.85,,S
292,1,1,"Bishop, Mrs. Dickinson H (Helen Walton)",female,19,1,0,11967,91.0792,B49,C
293,0,2,"Levy, Mr. Rene Jacques",male,36,0,0,SC/Paris 2163,12.875,D,C
294,0,3,"Haas, Miss. Aloisia",female,24,0,0,349236,8.85,,S
295,0,3,"Mineff, Mr. Ivan",male,24,0,0,349233,7.8958,,S
296,0,1,"Lewy, Mr. Ervin G",male,,0,0,PC 17612,27.7208,,C
297,0,3,"Hanna, Mr. Mansour",male,23.5,0,0,2693,7.2292,,C
298,0,1,"Allison, Miss. Helen Loraine",female,2,1,2,113781,151.55,C22 C26,S
299,1,1,"Saalfeld, Mr. Adolphe",male,,0,0,19988,30.5,C106,S
300,1,1,"Baxter, Mrs. James (Helene DeLaudeniere Chaput)",female,50,0,1,PC 17558,247.5208,B58 B60,C
301,1,3,"Kelly, Miss. Anna Katherine ""Annie Kate""",female,,0,0,9234,7.75,,Q
302,1,3,"McCoy, Mr. Bernard",male,,2,0,367226,23.25,,Q
303,0,3,"Johnson, Mr. William Cahoone Jr",male,19,0,0,LINE,0,,S
304,1,2,"Keane, Miss. Nora A",female,,0,0,226593,12.35,E101,Q
305,0,3,"Williams, Mr. Howard Hugh ""Harry""",male,,0,0,A/5 2466,8.05,,S
306,1,1,"Allison, Master. Hudson Trevor",male,0.92,1,2,113781,151.55,C22 C26,S
307,1,1,"Fleming, Miss. Margaret",female,,0,0,17421,110.8833,,C
308,1,1,"Penasco y Castellana, Mrs. Victor de Satode (Maria Josefa Perez de Soto y Vallejo)",female,17,1,0,PC 17758,108.9,C65,C
309,0,2,"Abelson, Mr. Samuel",male,30,1,0,P/PP 3381,24,,C
310,1,1,"Francatelli, Miss. Laura Mabel",female,30,0,0,PC 17485,56.9292,E36,C
311,1,1,"Hays, Miss. Margaret Bechstein",female,24,0,0,11767,83.1583,C54,C
312,1,1,"Ryerson, Miss. Emily Borie",female,18,2,2,PC 17608,262.375,B57 B59 B63 B66,C
313,0,2,"Lahtinen, Mrs. William (Anna Sylfven)",female,26,1,1,250651,26,,S
314,0,3,"Hendekovic, Mr. Ignjac",male,28,0,0,349243,7.8958,,S
315,0,2,"Hart, Mr. Benjamin",male,43,1,1,F.C.C. 13529,26.25,,S
316,1,3,"Nilsson, Miss. Helmina Josefina",female,26,0,0,347470,7.8542,,S
317,1,2,"Kantor, Mrs. Sinai (Miriam Sternin)",female,24,1,0,244367,26,,S
318,0,2,"Moraweck, Dr. Ernest",male,54,0,0,29011,14,,S
319,1,1,"Wick, Miss. Mary Natalie",female,31,0,2,36928,164.8667,C7,S
320,1,1,"Spedden, Mrs. Frederic Oakley (Margaretta Corning Stone)",female,40,1,1,16966,134.5,E34,C
321,0,3,"Dennis, Mr. Samuel",male,22,0,0,A/5 21172,7.25,,S
322,0,3,"Danoff, Mr. Yoto",male,27,0,0,349219,7.8958,,S
323,1,2,"Slayter, Miss. Hilda Mary",female,30,0,0,234818,12.35,,Q
324,1,2,"Caldwell, Mrs. Albert Francis (Sylvia Mae Harbaugh)",female,22,1,1,248738,29,,S
325,0,3,"Sage, Mr. George John Jr",male,,8,2,CA. 2343,69.55,,S
326,1,1,"Young, Miss. Marie Grice",female,36,0,0,PC 17760,135.6333,C32,C
327,0,3,"Nysveen, Mr. Johan Hansen",male,61,0,0,345364,6.2375,,S
328,1,2,"Ball, Mrs. (Ada E Hall)",female,36,0,0,28551,13,D,S
329,1,3,"Goldsmith, Mrs. Frank John (Emily Alice Brown)",female,31,1,1,363291,20.525,,S
330,1,1,"Hippach, Miss. Jean Gertrude",female,16,0,1,111361,57.9792,B18,C
331,1,3,"McCoy, Miss. Agnes",female,,2,0,367226,23.25,,Q
332,0,1,"Partner, Mr. Austen",male,45.5,0,0,113043,28.5,C124,S
333,0,1,"Graham, Mr. George Edward",male,38,0,1,PC 17582,153.4625,C91,S
334,0,3,"Vander Planke, Mr. Leo Edmondus",male,16,2,0,345764,18,,S
335,1,1,"Frauenthal, Mrs. Henry William (Clara Heinsheimer)",female,,1,0,PC 17611,133.65,,S
336,0,3,"Denkoff, Mr. Mitto",male,,0,0,349225,7.8958,,S
337,0,1,"Pears, Mr. Thomas Clinton",male,29,1,0,113776,66.6,C2,S
338,1,1,"Burns, Miss. Elizabeth Margaret",female,41,0,0,16966,134.5,E40,C
339,1,3,"Dahl, Mr. Karl Edwart",male,45,0,0,7598,8.05,,S
340,0,1,"Blackwell, Mr. Stephen Weart",male,45,0,0,113784,35.5,T,S
341,1,2,"Navratil, Master. Edmond Roger",male,2,1,1,230080,26,F2,S
342,1,1,"Fortune, Miss. Alice Elizabeth",female,24,3,2,19950,263,C23 C25 C27,S
343,0,2,"Collander, Mr. Erik Gustaf",male,28,0,0,248740,13,,S
344,0,2,"Sedgwick, Mr. Charles Frederick Waddington",male,25,0,0,244361,13,,S
345,0,2,"Fox, Mr. Stanley Hubert",male,36,0,0,229236,13,,S
346,1,2,"Brown, Miss. Amelia ""Mildred""",female,24,0,0,248733,13,F33,S
347,1,2,"Smith, Miss. Marion Elsie",female,40,0,0,31418,13,,S
348,1,3,"Davison, Mrs. Thomas Henry (Mary E Finck)",female,,1,0,386525,16.1,,S
349,1,3,"Coutts, Master. William Loch ""William""",male,3,1,1,C.A. 37671,15.9,,S
350,0,3,"Dimic, Mr. Jovan",male,42,0,0,315088,8.6625,,S
351,0,3,"Odahl, Mr. Nils Martin",male,23,0,0,7267,9.225,,S
352,0,1,"Williams-Lambert, Mr. Fletcher Fellows",male,,0,0,113510,35,C128,S
353,0,3,"Elias, Mr. Tannous",male,15,1,1,2695,7.2292,,C
354,0,3,"Arnold-Franchi, Mr. Josef",male,25,1,0,349237,17.8,,S
355,0,3,"Yousif, Mr. Wazli",male,,0,0,2647,7.225,,C
356,0,3,"Vanden Steen, Mr. Leo Peter",male,28,0,0,345783,9.5,,S
357,1,1,"Bowerman, Miss. Elsie Edith",female,22,0,1,113505,55,E33,S
358,0,2,"Funk, Miss. Annie Clemmer",female,38,0,0,237671,13,,S
359,1,3,"McGovern, Miss. Mary",female,,0,0,330931,7.8792,,Q
360,1,3,"Mockler, Miss. Helen Mary ""Ellie""",female,,0,0,330980,7.8792,,Q
361,0,3,"Skoog, Mr. Wilhelm",male,40,1,4,347088,27.9,,S
362,0,2,"del Carlo, Mr. Sebastiano",male,29,1,0,SC/PARIS 2167,27.7208,,C
363,0,3,"Barbara, Mrs. (Catherine David)",female,45,0,1,2691,14.4542,,C
364,0,3,"Asim, Mr. Adola",male,35,0,0,SOTON/O.Q. 3101310,7.05,,S
365,0,3,"O'Brien, Mr. Thomas",male,,1,0,370365,15.5,,Q
366,0,3,"Adahl, Mr. Mauritz Nils Martin",male,30,0,0,C 7076,7.25,,S
367,1,1,"Warren, Mrs. Frank Manley (Anna Sophia Atkinson)",female,60,1,0,110813,75.25,D37,C
368,1,3,"Moussa, Mrs. (Mantoura Boulos)",female,,0,0,2626,7.2292,,C
369,1,3,"Jermyn, Miss. Annie",female,,0,0,14313,7.75,,Q
370,1,1,"Aubart, Mme. Leontine Pauline",female,24,0,0,PC 17477,69.3,B35,C
371,1,1,"Harder, Mr. George Achilles",male,25,1,0,11765,55.4417,E50,C
372,0,3,"Wiklund, Mr. Jakob Alfred",male,18,1,0,3101267,6.4958,,S
373,0,3,"Beavan, Mr. William Thomas",male,19,0,0,323951,8.05,,S
374,0,1,"Ringhini, Mr. Sante",male,22,0,0,PC 17760,135.6333,,C
375,0,3,"Palsson, Miss. Stina Viola",female,3,3,1,349909,21.075,,S
376,1,1,"Meyer, Mrs. Edgar Joseph (Leila Saks)",female,,1,0,PC 17604,82.1708,,C
377,1,3,"Landergren, Miss. Aurora Adelia",female,22,0,0,C 7077,7.25,,S
378,0,1,"Widener, Mr. Harry Elkins",male,27,0,2,113503,211.5,C82,C
379,0,3,"Betros, Mr. Tannous",male,20,0,0,2648,4.0125,,C
380,0,3,"Gustafsson, Mr. Karl Gideon",male,19,0,0,347069,7.775,,S
381,1,1,"Bidois, Miss. Rosalie",female,42,0,0,PC 17757,227.525,,C
382,1,3,"Nakid, Miss. Maria (""Mary"")",female,1,0,2,2653,15.7417,,C
383,0,3,"Tikkanen, Mr. Juho",male,32,0,0,STON/O 2. 3101293,7.925,,S
384,1,1,"Holverson, Mrs. Alexander Oskar (Mary Aline Towner)",female,35,1,0,113789,52,,S
385,0,3,"Plotcharsky, Mr. Vasil",male,,0,0,349227,7.8958,,S
386,0,2,"Davies, Mr. Charles Henry",male,18,0,0,S.O.C. 14879,73.5,,S
387,0,3,"Goodwin, Master. Sidney Leonard",male,1,5,2,CA 2144,46.9,,S
388,1,2,"Buss, Miss. Kate",female,36,0,0,27849,13,,S
389,0,3,"Sadlier, Mr. Matthew",male,,0,0,367655,7.7292,,Q
390,1,2,"Lehmann, Miss. Bertha",female,17,0,0,SC 1748,12,,C
391,1,1,"Carter, Mr. William Ernest",male,36,1,2,113760,120,B96 B98,S
392,1,3,"Jansson, Mr. Carl Olof",male,21,0,0,350034,7.7958,,S
393,0,3,"Gustafsson, Mr. Johan Birger",male,28,2,0,3101277,7.925,,S
394,1,1,"Newell, Miss. Marjorie",female,23,1,0,35273,113.275,D36,C
395,1,3,"Sandstrom, Mrs. Hjalmar (Agnes Charlotta Bengtsson)",female,24,0,2,PP 9549,16.7,G6,S
396,0,3,"Johansson, Mr. Erik",male,22,0,0,350052,7.7958,,S
397,0,3,"Olsson, Miss. Elina",female,31,0,0,350407,7.8542,,S
398,0,2,"McKane, Mr. Peter David",male,46,0,0,28403,26,,S
399,0,2,"Pain, Dr. Alfred",male,23,0,0,244278,10.5,,S
400,1,2,"Trout, Mrs. William H (Jessie L)",female,28,0,0,240929,12.65,,S
401,1,3,"Niskanen, Mr. Juha",male,39,0,0,STON/O 2. 3101289,7.925,,S
402,0,3,"Adams, Mr. John",male,26,0,0,341826,8.05,,S
403,0,3,"Jussila, Miss. Mari Aina",female,21,1,0,4137,9.825,,S
404,0,3,"Hakkarainen, Mr. Pekka Pietari",male,28,1,0,STON/O2. 3101279,15.85,,S
405,0,3,"Oreskovic, Miss. Marija",female,20,0,0,315096,8.6625,,S
406,0,2,"Gale, Mr. Shadrach",male,34,1,0,28664,21,,S
407,0,3,"Widegren, Mr. Carl/Charles Peter",male,51,0,0,347064,7.75,,S
408,1,2,"Richards, Master. William Rowe",male,3,1,1,29106,18.75,,S
409,0,3,"Birkeland, Mr. Hans Martin Monsen",male,21,0,0,312992,7.775,,S
410,0,3,"Lefebre, Miss. Ida",female,,3,1,4133,25.4667,,S
411,0,3,"Sdycoff, Mr. Todor",male,,0,0,349222,7.8958,,S
412,0,3,"Hart, Mr. Henry",male,,0,0,394140,6.8583,,Q
413,1,1,"Minahan, Miss. Daisy E",female,33,1,0,19928,90,C78,Q
414,0,2,"Cunningham, Mr. Alfred Fleming",male,,0,0,239853,0,,S
415,1,3,"Sundman, Mr. Johan Julian",male,44,0,0,STON/O 2. 3101269,7.925,,S
416,0,3,"Meek, Mrs. Thomas (Annie Louise Rowley)",female,,0,0,343095,8.05,,S
417,1,2,"Drew, Mrs. James Vivian (Lulu Thorne Christian)",female,34,1,1,28220,32.5,,S
418,1,2,"Silven, Miss. Lyyli Karoliina",female,18,0,2,250652,13,,S
419,0,2,"Matthews, Mr. William John",male,30,0,0,28228,13,,S
420,0,3,"Van Impe, Miss. Catharina",female,10,0,2,345773,24.15,,S
421,0,3,"Gheorgheff, Mr. Stanio",male,,0,0,349254,7.8958,,C
422,0,3,"Charters, Mr. David",male,21,0,0,A/5. 13032,7.7333,,Q
423,0,3,"Zimmerman, Mr. Leo",male,29,0,0,315082,7.875,,S
424,0,3,"Danbom, Mrs. Ernst Gilbert (Anna Sigrid Maria Brogren)",female,28,1,1,347080,14.4,,S
425,0,3,"Rosblom, Mr. Viktor Richard",male,18,1,1,370129,20.2125,,S
426,0,3,"Wiseman, Mr. Phillippe",male,,0,0,A/4. 34244,7.25,,S
427,1,2,"Clarke, Mrs. Charles V (Ada Maria Winfield)",female,28,1,0,2003,26,,S
428,1,2,"Phillips, Miss. Kate Florence (""Mrs Kate Louise Phillips Marshall"")",female,19,0,0,250655,26,,S
429,0,3,"Flynn, Mr. James",male,,0,0,364851,7.75,,Q
430,1,3,"Pickard, Mr. Berk (Berk Trembisky)",male,32,0,0,SOTON/O.Q. 392078,8.05,E10,S
431,1,1,"Bjornstrom-Steffansson, Mr. Mauritz Hakan",male,28,0,0,110564,26.55,C52,S
432,1,3,"Thorneycroft, Mrs. Percival (Florence Kate White)",female,,1,0,376564,16.1,,S
433,1,2,"Louch, Mrs. Charles Alexander (Alice Adelaide Slow)",female,42,1,0,SC/AH 3085,26,,S
434,0,3,"Kallio, Mr. Nikolai Erland",male,17,0,0,STON/O 2. 3101274,7.125,,S
435,0,1,"Silvey, Mr. William Baird",male,50,1,0,13507,55.9,E44,S
436,1,1,"Carter, Miss. Lucile Polk",female,14,1,2,113760,120,B96 B98,S
437,0,3,"Ford, Miss. Doolina Margaret ""Daisy""",female,21,2,2,W./C. 6608,34.375,,S
438,1,2,"Richards, Mrs. Sidney (Emily Hocking)",female,24,2,3,29106,18.75,,S
439,0,1,"Fortune, Mr. Mark",male,64,1,4,19950,263,C23 C25 C27,S
440,0,2,"Kvillner, Mr. Johan Henrik Johannesson",male,31,0,0,C.A. 18723,10.5,,S
441,1,2,"Hart, Mrs. Benjamin (Esther Ada Bloomfield)",female,45,1,1,F.C.C. 13529,26.25,,S
442,0,3,"Hampe, Mr. Leon",male,20,0,0,345769,9.5,,S
443,0,3,"Petterson, Mr. Johan Emil",male,25,1,0,347076,7.775,,S
444,1,2,"Reynaldo, Ms. Encarnacion",female,28,0,0,230434,13,,S
445,1,3,"Johannesen-Bratthammer, Mr. Bernt",male,,0,0,65306,8.1125,,S
446,1,1,"Dodge, Master. Washington",male,4,0,2,33638,81.8583,A34,S
447,1,2,"Mellinger, Miss. Madeleine Violet",female,13,0,1,250644,19.5,,S
448,1,1,"Seward, Mr. Frederic Kimber",male,34,0,0,113794,26.55,,S
449,1,3,"Baclini, Miss. Marie Catherine",female,5,2,1,2666,19.2583,,C
450,1,1,"Peuchen, Major. Arthur Godfrey",male,52,0,0,113786,30.5,C104,S
451,0,2,"West, Mr. Edwy Arthur",male,36,1,2,C.A. 34651,27.75,,S
452,0,3,"Hagland, Mr. Ingvald Olai Olsen",male,,1,0,65303,19.9667,,S
453,0,1,"Foreman, Mr. Benjamin Laventall",male,30,0,0,113051,27.75,C111,C
454,1,1,"Goldenberg, Mr. Samuel L",male,49,1,0,17453,89.1042,C92,C
455,0,3,"Peduzzi, Mr. Joseph",male,,0,0,A/5 2817,8.05,,S
456,1,3,"Jalsevac, Mr. Ivan",male,29,0,0,349240,7.8958,,C
457,0,1,"Millet, Mr. Francis Davis",male,65,0,0,13509,26.55,E38,S
458,1,1,"Kenyon, Mrs. Frederick R (Marion)",female,,1,0,17464,51.8625,D21,S
459,1,2,"Toomey, Miss. Ellen",female,50,0,0,F.C.C. 13531,10.5,,S
460,0,3,"O'Connor, Mr. Maurice",male,,0,0,371060,7.75,,Q
461,1,1,"Anderson, Mr. Harry",male,48,0,0,19952,26.55,E12,S
462,0,3,"Morley, Mr. William",male,34,0,0,364506,8.05,,S
463,0,1,"Gee, Mr. Arthur H",male,47,0,0,111320,38.5,E63,S
464,0,2,"Milling, Mr. Jacob Christian",male,48,0,0,234360,13,,S
465,0,3,"Maisner, Mr. Simon",male,,0,0,A/S 2816,8.05,,S
466,0,3,"Goncalves, Mr. Manuel Estanslas",male,38,0,0,SOTON/O.Q. 3101306,7.05,,S
467,0,2,"Campbell, Mr. William",male,,0,0,239853,0,,S
468,0,1,"Smart, Mr. John Montgomery",male,56,0,0,113792,26.55,,S
469,0,3,"Scanlan, Mr. James",male,,0,0,36209,7.725,,Q
470,1,3,"Baclini, Miss. Helene Barbara",female,0.75,2,1,2666,19.2583,,C
471,0,3,"Keefe, Mr. Arthur",male,,0,0,323592,7.25,,S
472,0,3,"Cacic, Mr. Luka",male,38,0,0,315089,8.6625,,S
473,1,2,"West, Mrs. Edwy Arthur (Ada Mary Worth)",female,33,1,2,C.A. 34651,27.75,,S
474,1,2,"Jerwan, Mrs. Amin S (Marie Marthe Thuillard)",female,23,0,0,SC/AH Basle 541,13.7917,D,C
475,0,3,"Strandberg, Miss. Ida Sofia",female,22,0,0,7553,9.8375,,S
476,0,1,"Clifford, Mr. George Quincy",male,,0,0,110465,52,A14,S
477,0,2,"Renouf, Mr. Peter Henry",male,34,1,0,31027,21,,S
478,0,3,"Braund, Mr. Lewis Richard",male,29,1,0,3460,7.0458,,S
479,0,3,"Karlsson, Mr. Nils August",male,22,0,0,350060,7.5208,,S
480,1,3,"Hirvonen, Miss. Hildur E",female,2,0,1,3101298,12.2875,,S
481,0,3,"Goodwin, Master. Harold Victor",male,9,5,2,CA 2144,46.9,,S
482,0,2,"Frost, Mr. Anthony Wood ""Archie""",male,,0,0,239854,0,,S
483,0,3,"Rouse, Mr. Richard Henry",male,50,0,0,A/5 3594,8.05,,S
484,1,3,"Turkula, Mrs. (Hedwig)",female,63,0,0,4134,9.5875,,S
485,1,1,"Bishop, Mr. Dickinson H",male,25,1,0,11967,91.0792,B49,C
486,0,3,"Lefebre, Miss. Jeannie",female,,3,1,4133,25.4667,,S
487,1,1,"Hoyt, Mrs. Frederick Maxfield (Jane Anne Forby)",female,35,1,0,19943,90,C93,S
488,0,1,"Kent, Mr. Edward Austin",male,58,0,0,11771,29.7,B37,C
489,0,3,"Somerton, Mr. Francis William",male,30,0,0,A.5. 18509,8.05,,S
490,1,3,"Coutts, Master. Eden Leslie ""Neville""",male,9,1,1,C.A. 37671,15.9,,S
491,0,3,"Hagland, Mr. Konrad Mathias Reiersen",male,,1,0,65304,19.9667,,S
492,0,3,"Windelov, Mr. Einar",male,21,0,0,SOTON/OQ 3101317,7.25,,S
493,0,1,"Molson, Mr. Harry Markland",male,55,0,0,113787,30.5,C30,S
494,0,1,"Artagaveytia, Mr. Ramon",male,71,0,0,PC 17609,49.5042,,C
495,0,3,"Stanley, Mr. Edward Roland",male,21,0,0,A/4 45380,8.05,,S
496,0,3,"Yousseff, Mr. Gerious",male,,0,0,2627,14.4583,,C
497,1,1,"Eustis, Miss. Elizabeth Mussey",female,54,1,0,36947,78.2667,D20,C
498,0,3,"Shellard, Mr. Frederick William",male,,0,0,C.A. 6212,15.1,,S
499,0,1,"Allison, Mrs. Hudson J C (Bessie Waldo Daniels)",female,25,1,2,113781,151.55,C22 C26,S
500,0,3,"Svensson, Mr. Olof",male,24,0,0,350035,7.7958,,S
501,0,3,"Calic, Mr. Petar",male,17,0,0,315086,8.6625,,S
502,0,3,"Canavan, Miss. Mary",female,21,0,0,364846,7.75,,Q
503,0,3,"O'Sullivan, Miss. Bridget Mary",female,,0,0,330909,7.6292,,Q
504,0,3,"Laitinen, Miss. Kristina Sofia",female,37,0,0,4135,9.5875,,S
505,1,1,"Maioni, Miss. Roberta",female,16,0,0,110152,86.5,B79,S
506,0,1,"Penasco y Castellana, Mr. Victor de Satode",male,18,1,0,PC 17758,108.9,C65,C
507,1,2,"Quick, Mrs. Frederick Charles (Jane Richards)",female,33,0,2,26360,26,,S
508,1,1,"Bradley, Mr. George (""George Arthur Brayton"")",male,,0,0,111427,26.55,,S
509,0,3,"Olsen, Mr. Henry Margido",male,28,0,0,C 4001,22.525,,S
510,1,3,"Lang, Mr. Fang",male,26,0,0,1601,56.4958,,S
511,1,3,"Daly, Mr. Eugene Patrick",male,29,0,0,382651,7.75,,Q
512,0,3,"Webber, Mr. James",male,,0,0,SOTON/OQ 3101316,8.05,,S
513,1,1,"McGough, Mr. James Robert",male,36,0,0,PC 17473,26.2875,E25,S
514,1,1,"Rothschild, Mrs. Martin (Elizabeth L. Barrett)",female,54,1,0,PC 17603,59.4,,C
515,0,3,"Coleff, Mr. Satio",male,24,0,0,349209,7.4958,,S
516,0,1,"Walker, Mr. William Anderson",male,47,0,0,36967,34.0208,D46,S
517,1,2,"Lemore, Mrs. (Amelia Milley)",female,34,0,0,C.A. 34260,10.5,F33,S
518,0,3,"Ryan, Mr. Patrick",male,,0,0,371110,24.15,,Q
519,1,2,"Angle, Mrs. William A (Florence ""Mary"" Agnes Hughes)",female,36,1,0,226875,26,,S
520,0,3,"Pavlovic, Mr. Stefo",male,32,0,0,349242,7.8958,,S
521,1,1,"Perreault, Miss. Anne",female,30,0,0,12749,93.5,B73,S
522,0,3,"Vovk, Mr. Janko",male,22,0,0,349252,7.8958,,S
523,0,3,"Lahoud, Mr. Sarkis",male,,0,0,2624,7.225,,C
524,1,1,"Hippach, Mrs. Louis Albert (Ida Sophia Fischer)",female,44,0,1,111361,57.9792,B18,C
525,0,3,"Kassem, Mr. Fared",male,,0,0,2700,7.2292,,C
526,0,3,"Farrell, Mr. James",male,40.5,0,0,367232,7.75,,Q
527,1,2,"Ridsdale, Miss. Lucy",female,50,0,0,W./C. 14258,10.5,,S
528,0,1,"Farthing, Mr. John",male,,0,0,PC 17483,221.7792,C95,S
529,0,3,"Salonen, Mr. Johan Werner",male,39,0,0,3101296,7.925,,S
530,0,2,"Hocking, Mr. Richard George",male,23,2,1,29104,11.5,,S
531,1,2,"Quick, Miss. Phyllis May",female,2,1,1,26360,26,,S
532,0,3,"Toufik, Mr. Nakli",male,,0,0,2641,7.2292,,C
533,0,3,"Elias, Mr. Joseph Jr",male,17,1,1,2690,7.2292,,C
534,1,3,"Peter, Mrs. Catherine (Catherine Rizk)",female,,0,2,2668,22.3583,,C
535,0,3,"Cacic, Miss. Marija",female,30,0,0,315084,8.6625,,S
536,1,2,"Hart, Miss. Eva Miriam",female,7,0,2,F.C.C. 13529,26.25,,S
537,0,1,"Butt, Major. Archibald Willingham",male,45,0,0,113050,26.55,B38,S
538,1,1,"LeRoy, Miss. Bertha",female,30,0,0,PC 17761,106.425,,C
539,0,3,"Risien, Mr. Samuel Beard",male,,0,0,364498,14.5,,S
540,1,1,"Frolicher, Miss. Hedwig Margaritha",female,22,0,2,13568,49.5,B39,C
541,1,1,"Crosby, Miss. Harriet R",female,36,0,2,WE/P 5735,71,B22,S
542,0,3,"Andersson, Miss. Ingeborg Constanzia",female,9,4,2,347082,31.275,,S
543,0,3,"Andersson, Miss. Sigrid Elisabeth",female,11,4,2,347082,31.275,,S
544,1,2,"Beane, Mr. Edward",male,32,1,0,2908,26,,S
545,0,1,"Douglas, Mr. Walter Donald",male,50,1,0,PC 17761,106.425,C86,C
546,0,1,"Nicholson, Mr. Arthur Ernest",male,64,0,0,693,26,,S
547,1,2,"Beane, Mrs. Edward (Ethel Clarke)",female,19,1,0,2908,26,,S
548,1,2,"Padro y Manent, Mr. Julian",male,,0,0,SC/PARIS 2146,13.8625,,C
549,0,3,"Goldsmith, Mr. Frank John",male,33,1,1,363291,20.525,,S
550,1,2,"Davies, Master. John Morgan Jr",male,8,1,1,C.A. 33112,36.75,,S
551,1,1,"Thayer, Mr. John Borland Jr",male,17,0,2,17421,110.8833,C70,C
552,0,2,"Sharp, Mr. Percival James R",male,27,0,0,244358,26,,S
553,0,3,"O'Brien, Mr. Timothy",male,,0,0,330979,7.8292,,Q
554,1,3,"Leeni, Mr. Fahim (""Philip Zenni"")",male,22,0,0,2620,7.225,,C
555,1,3,"Ohman, Miss. Velin",female,22,0,0,347085,7.775,,S
556,0,1,"Wright, Mr. George",male,62,0,0,113807,26.55,,S
557,1,1,"Duff Gordon, Lady. (Lucille Christiana Sutherland) (""Mrs Morgan"")",female,48,1,0,11755,39.6,A16,C
558,0,1,"Robbins, Mr. Victor",male,,0,0,PC 17757,227.525,,C
559,1,1,"Taussig, Mrs. Emil (Tillie Mandelbaum)",female,39,1,1,110413,79.65,E67,S
560,1,3,"de Messemaeker, Mrs. Guillaume Joseph (Emma)",female,36,1,0,345572,17.4,,S
561,0,3,"Morrow, Mr. Thomas Rowan",male,,0,0,372622,7.75,,Q
562,0,3,"Sivic, Mr. Husein",male,40,0,0,349251,7.8958,,S
563,0,2,"Norman, Mr. Robert Douglas",male,28,0,0,218629,13.5,,S
564,0,3,"Simmons, Mr. John",male,,0,0,SOTON/OQ 392082,8.05,,S
565,0,3,"Meanwell, Miss. (Marion Ogden)",female,,0,0,SOTON/O.Q. 392087,8.05,,S
566,0,3,"Davies, Mr. Alfred J",male,24,2,0,A/4 48871,24.15,,S
567,0,3,"Stoytcheff, Mr. Ilia",male,19,0,0,349205,7.8958,,S
568,0,3,"Palsson, Mrs. Nils (Alma Cornelia Berglund)",female,29,0,4,349909,21.075,,S
569,0,3,"Doharr, Mr. Tannous",male,,0,0,2686,7.2292,,C
570,1,3,"Jonsson, Mr. Carl",male,32,0,0,350417,7.8542,,S
571,1,2,"Harris, Mr. George",male,62,0,0,S.W./PP 752,10.5,,S
572,1,1,"Appleton, Mrs. Edward Dale (Charlotte Lamson)",female,53,2,0,11769,51.4792,C101,S
573,1,1,"Flynn, Mr. John Irwin (""Irving"")",male,36,0,0,PC 17474,26.3875,E25,S
574,1,3,"Kelly, Miss. Mary",female,,0,0,14312,7.75,,Q
575,0,3,"Rush, Mr. Alfred George John",male,16,0,0,A/4. 20589,8.05,,S
576,0,3,"Patchett, Mr. George",male,19,0,0,358585,14.5,,S
577,1,2,"Garside, Miss. Ethel",female,34,0,0,243880,13,,S
578,1,1,"Silvey, Mrs. William Baird (Alice Munger)",female,39,1,0,13507,55.9,E44,S
579,0,3,"Caram, Mrs. Joseph (Maria Elias)",female,,1,0,2689,14.4583,,C
580,1,3,"Jussila, Mr. Eiriik",male,32,0,0,STON/O 2. 3101286,7.925,,S
581,1,2,"Christy, Miss. Julie Rachel",female,25,1,1,237789,30,,S
582,1,1,"Thayer, Mrs. John Borland (Marian Longstreth Morris)",female,39,1,1,17421,110.8833,C68,C
583,0,2,"Downton, Mr. William James",male,54,0,0,28403,26,,S
584,0,1,"Ross, Mr. John Hugo",male,36,0,0,13049,40.125,A10,C
585,0,3,"Paulner, Mr. Uscher",male,,0,0,3411,8.7125,,C
586,1,1,"Taussig, Miss. Ruth",female,18,0,2,110413,79.65,E68,S
587,0,2,"Jarvis, Mr. John Denzil",male,47,0,0,237565,15,,S
588,1,1,"Frolicher-Stehli, Mr. Maxmillian",male,60,1,1,13567,79.2,B41,C
589,0,3,"Gilinski, Mr. Eliezer",male,22,0,0,14973,8.05,,S
590,0,3,"Murdlin, Mr. Joseph",male,,0,0,A./5. 3235,8.05,,S
591,0,3,"Rintamaki, Mr. Matti",male,35,0,0,STON/O 2. 3101273,7.125,,S
592,1,1,"Stephenson, Mrs. Walter Bertram (Martha Eustis)",female,52,1,0,36947,78.2667,D20,C
593,0,3,"Elsbury, Mr. William James",male,47,0,0,A/5 3902,7.25,,S
594,0,3,"Bourke, Miss. Mary",female,,0,2,364848,7.75,,Q
595,0,2,"Chapman, Mr. John Henry",male,37,1,0,SC/AH 29037,26,,S
596,0,3,"Van Impe, Mr. Jean Baptiste",male,36,1,1,345773,24.15,,S
597,1,2,"Leitch, Miss. Jessie Wills",female,,0,0,248727,33,,S
598,0,3,"Johnson, Mr. Alfred",male,49,0,0,LINE,0,,S
599,0,3,"Boulos, Mr. Hanna",male,,0,0,2664,7.225,,C
600,1,1,"Duff Gordon, Sir. Cosmo Edmund (""Mr Morgan"")",male,49,1,0,PC 17485,56.9292,A20,C
601,1,2,"Jacobsohn, Mrs. Sidney Samuel (Amy Frances Christy)",female,24,2,1,243847,27,,S
602,0,3,"Slabenoff, Mr. Petco",male,,0,0,349214,7.8958,,S
603,0,1,"Harrington, Mr. Charles H",male,,0,0,113796,42.4,,S
604,0,3,"Torber, Mr. Ernst William",male,44,0,0,364511,8.05,,S
605,1,1,"Homer, Mr. Harry (""Mr E Haven"")",male,35,0,0,111426,26.55,,C
606,0,3,"Lindell, Mr. Edvard Bengtsson",male,36,1,0,349910,15.55,,S
607,0,3,"Karaic, Mr. Milan",male,30,0,0,349246,7.8958,,S
608,1,1,"Daniel, Mr. Robert Williams",male,27,0,0,113804,30.5,,S
609,1,2,"Laroche, Mrs. Joseph (Juliette Marie Louise Lafargue)",female,22,1,2,SC/Paris 2123,41.5792,,C
610,1,1,"Shutes, Miss. Elizabeth W",female,40,0,0,PC 17582,153.4625,C125,S
611,0,3,"Andersson, Mrs. Anders Johan (Alfrida Konstantia Brogren)",female,39,1,5,347082,31.275,,S
612,0,3,"Jardin, Mr. Jose Neto",male,,0,0,SOTON/O.Q. 3101305,7.05,,S
613,1,3,"Murphy, Miss. Margaret Jane",female,,1,0,367230,15.5,,Q
614,0,3,"Horgan, Mr. John",male,,0,0,370377,7.75,,Q
615,0,3,"Brocklebank, Mr. William Alfred",male,35,0,0,364512,8.05,,S
616,1,2,"Herman, Miss. Alice",female,24,1,2,220845,65,,S
617,0,3,"Danbom, Mr. Ernst Gilbert",male,34,1,1,347080,14.4,,S
618,0,3,"Lobb, Mrs. William Arthur (Cordelia K Stanlick)",female,26,1,0,A/5. 3336,16.1,,S
619,1,2,"Becker, Miss. Marion Louise",female,4,2,1,230136,39,F4,S
620,0,2,"Gavey, Mr. Lawrence",male,26,0,0,31028,10.5,,S
621,0,3,"Yasbeck, Mr. Antoni",male,27,1,0,2659,14.4542,,C
622,1,1,"Kimball, Mr. Edwin Nelson Jr",male,42,1,0,11753,52.5542,D19,S
623,1,3,"Nakid, Mr. Sahid",male,20,1,1,2653,15.7417,,C
624,0,3,"Hansen, Mr. Henry Damsgaard",male,21,0,0,350029,7.8542,,S
625,0,3,"Bowen, Mr. David John ""Dai""",male,21,0,0,54636,16.1,,S
626,0,1,"Sutton, Mr. Frederick",male,61,0,0,36963,32.3208,D50,S
627,0,2,"Kirkland, Rev. Charles Leonard",male,57,0,0,219533,12.35,,Q
628,1,1,"Longley, Miss. Gretchen Fiske",female,21,0,0,13502,77.9583,D9,S
629,0,3,"Bostandyeff, Mr. Guentcho",male,26,0,0,349224,7.8958,,S
630,0,3,"O'Connell, Mr. Patrick D",male,,0,0,334912,7.7333,,Q
631,1,1,"Barkworth, Mr. Algernon Henry Wilson",male,80,0,0,27042,30,A23,S
632,0,3,"Lundahl, Mr. Johan Svensson",male,51,0,0,347743,7.0542,,S
633,1,1,"Stahelin-Maeglin, Dr. Max",male,32,0,0,13214,30.5,B50,C
634,0,1,"Parr, Mr. William Henry Marsh",male,,0,0,112052,0,,S
635,0,3,"Skoog, Miss. Mabel",female,9,3,2,347088,27.9,,S
636,1,2,"Davis, Miss. Mary",female,28,0,0,237668,13,,S
637,0,3,"Leinonen, Mr. Antti Gustaf",male,32,0,0,STON/O 2. 3101292,7.925,,S
638,0,2,"Collyer, Mr. Harvey",male,31,1,1,C.A. 31921,26.25,,S
639,0,3,"Panula, Mrs. Juha (Maria Emilia Ojala)",female,41,0,5,3101295,39.6875,,S
640,0,3,"Thorneycroft, Mr. Percival",male,,1,0,376564,16.1,,S
641,0,3,"Jensen, Mr. Hans Peder",male,20,0,0,350050,7.8542,,S
642,1,1,"Sagesser, Mlle. Emma",female,24,0,0,PC 17477,69.3,B35,C
643,0,3,"Skoog, Miss. Margit Elizabeth",female,2,3,2,347088,27.9,,S
644,1,3,"Foo, Mr. Choong",male,,0,0,1601,56.4958,,S
645,1,3,"Baclini, Miss. Eugenie",female,0.75,2,1,2666,19.2583,,C
646,1,1,"Harper, Mr. Henry Sleeper",male,48,1,0,PC 17572,76.7292,D33,C
647,0,3,"Cor, Mr. Liudevit",male,19,0,0,349231,7.8958,,S
648,1,1,"Simonius-Blumer, Col. Oberst Alfons",male,56,0,0,13213,35.5,A26,C
649,0,3,"Willey, Mr. Edward",male,,0,0,S.O./P.P. 751,7.55,,S
650,1,3,"Stanley, Miss. Amy Zillah Elsie",female,23,0,0,CA. 2314,7.55,,S
651,0,3,"Mitkoff, Mr. Mito",male,,0,0,349221,7.8958,,S
652,1,2,"Doling, Miss. Elsie",female,18,0,1,231919,23,,S
653,0,3,"Kalvik, Mr. Johannes Halvorsen",male,21,0,0,8475,8.4333,,S
654,1,3,"O'Leary, Miss. Hanora ""Norah""",female,,0,0,330919,7.8292,,Q
655,0,3,"Hegarty, Miss. Hanora ""Nora""",female,18,0,0,365226,6.75,,Q
656,0,2,"Hickman, Mr. Leonard Mark",male,24,2,0,S.O.C. 14879,73.5,,S
657,0,3,"Radeff, Mr. Alexander",male,,0,0,349223,7.8958,,S
658,0,3,"Bourke, Mrs. John (Catherine)",female,32,1,1,364849,15.5,,Q
659,0,2,"Eitemiller, Mr. George Floyd",male,23,0,0,29751,13,,S
660,0,1,"Newell, Mr. Arthur Webster",male,58,0,2,35273,113.275,D48,C
661,1,1,"Frauenthal, Dr. Henry William",male,50,2,0,PC 17611,133.65,,S
662,0,3,"Badt, Mr. Mohamed",male,40,0,0,2623,7.225,,C
663,0,1,"Colley, Mr. Edward Pomeroy",male,47,0,0,5727,25.5875,E58,S
664,0,3,"Coleff, Mr. Peju",male,36,0,0,349210,7.4958,,S
665,1,3,"Lindqvist, Mr. Eino William",male,20,1,0,STON/O 2. 3101285,7.925,,S
666,0,2,"Hickman, Mr. Lewis",male,32,2,0,S.O.C. 14879,73.5,,S
667,0,2,"Butler, Mr. Reginald Fenton",male,25,0,0,234686,13,,S
668,0,3,"Rommetvedt, Mr. Knud Paust",male,,0,0,312993,7.775,,S
669,0,3,"Cook, Mr. Jacob",male,43,0,0,A/5 3536,8.05,,S
670,1,1,"Taylor, Mrs. Elmer Zebley (Juliet Cummins Wright)",female,,1,0,19996,52,C126,S
671,1,2,"Brown, Mrs. Thomas William Solomon (Elizabeth Catherine Ford)",female,40,1,1,29750,39,,S
672,0,1,"Davidson, Mr. Thornton",male,31,1,0,F.C. 12750,52,B71,S
673,0,2,"Mitchell, Mr. Henry Michael",male,70,0,0,C.A. 24580,10.5,,S
674,1,2,"Wilhelms, Mr. Charles",male,31,0,0,244270,13,,S
675,0,2,"Watson, Mr. Ennis Hastings",male,,0,0,239856,0,,S
676,0,3,"Edvardsson, Mr. Gustaf Hjalmar",male,18,0,0,349912,7.775,,S
677,0,3,"Sawyer, Mr. Frederick Charles",male,24.5,0,0,342826,8.05,,S
678,1,3,"Turja, Miss. Anna Sofia",female,18,0,0,4138,9.8417,,S
679,0,3,"Goodwin, Mrs. Frederick (Augusta Tyler)",female,43,1,6,CA 2144,46.9,,S
680,1,1,"Cardeza, Mr. Thomas Drake Martinez",male,36,0,1,PC 17755,512.3292,B51 B53 B55,C
681,0,3,"Peters, Miss. Katie",female,,0,0,330935,8.1375,,Q
682,1,1,"Hassab, Mr. Hammad",male,27,0,0,PC 17572,76.7292,D49,C
683,0,3,"Olsvigen, Mr. Thor Anderson",male,20,0,0,6563,9.225,,S
684,0,3,"Goodwin, Mr. Charles Edward",male,14,5,2,CA 2144,46.9,,S
685,0,2,"Brown, Mr. Thomas William Solomon",male,60,1,1,29750,39,,S
686,0,2,"Laroche, Mr. Joseph Philippe Lemercier",male,25,1,2,SC/Paris 2123,41.5792,,C
687,0,3,"Panula, Mr. Jaako Arnold",male,14,4,1,3101295,39.6875,,S
688,0,3,"Dakic, Mr. Branko",male,19,0,0,349228,10.1708,,S
689,0,3,"Fischer, Mr. Eberhard Thelander",male,18,0,0,350036,7.7958,,S
690,1,1,"Madill, Miss. Georgette Alexandra",female,15,0,1,24160,211.3375,B5,S
691,1,1,"Dick, Mr. Albert Adrian",male,31,1,0,17474,57,B20,S
692,1,3,"Karun, Miss. Manca",female,4,0,1,349256,13.4167,,C
693,1,3,"Lam, Mr. Ali",male,,0,0,1601,56.4958,,S
694,0,3,"Saad, Mr. Khalil",male,25,0,0,2672,7.225,,C
695,0,1,"Weir, Col. John",male,60,0,0,113800,26.55,,S
696,0,2,"Chapman, Mr. Charles Henry",male,52,0,0,248731,13.5,,S
697,0,3,"Kelly, Mr. James",male,44,0,0,363592,8.05,,S
698,1,3,"Mullens, Miss. Katherine ""Katie""",female,,0,0,35852,7.7333,,Q
699,0,1,"Thayer, Mr. John Borland",male,49,1,1,17421,110.8833,C68,C
700,0,3,"Humblen, Mr. Adolf Mathias Nicolai Olsen",male,42,0,0,348121,7.65,F G63,S
701,1,1,"Astor, Mrs. John Jacob (Madeleine Talmadge Force)",female,18,1,0,PC 17757,227.525,C62 C64,C
702,1,1,"Silverthorne, Mr. Spencer Victor",male,35,0,0,PC 17475,26.2875,E24,S
703,0,3,"Barbara, Miss. Saiide",female,18,0,1,2691,14.4542,,C
704,0,3,"Gallagher, Mr. Martin",male,25,0,0,36864,7.7417,,Q
705,0,3,"Hansen, Mr. Henrik Juul",male,26,1,0,350025,7.8542,,S
706,0,2,"Morley, Mr. Henry Samuel (""Mr Henry Marshall"")",male,39,0,0,250655,26,,S
707,1,2,"Kelly, Mrs. Florence ""Fannie""",female,45,0,0,223596,13.5,,S
708,1,1,"Calderhead, Mr. Edward Pennington",male,42,0,0,PC 17476,26.2875,E24,S
709,1,1,"Cleaver, Miss. Alice",female,22,0,0,113781,151.55,,S
710,1,3,"Moubarek, Master. Halim Gonios (""William George"")",male,,1,1,2661,15.2458,,C
711,1,1,"Mayne, Mlle. Berthe Antonine (""Mrs de Villiers"")",female,24,0,0,PC 17482,49.5042,C90,C
712,0,1,"Klaber, Mr. Herman",male,,0,0,113028,26.55,C124,S
713,1,1,"Taylor, Mr. Elmer Zebley",male,48,1,0,19996,52,C126,S
714,0,3,"Larsson, Mr. August Viktor",male,29,0,0,7545,9.4833,,S
715,0,2,"Greenberg, Mr. Samuel",male,52,0,0,250647,13,,S
716,0,3,"Soholt, Mr. Peter Andreas Lauritz Andersen",male,19,0,0,348124,7.65,F G73,S
717,1,1,"Endres, Miss. Caroline Louise",female,38,0,0,PC 17757,227.525,C45,C
718,1,2,"Troutt, Miss. Edwina Celia ""Winnie""",female,27,0,0,34218,10.5,E101,S
719,0,3,"McEvoy, Mr. Michael",male,,0,0,36568,15.5,,Q
720,0,3,"Johnson, Mr. Malkolm Joackim",male,33,0,0,347062,7.775,,S
721,1,2,"Harper, Miss. Annie Jessie ""Nina""",female,6,0,1,248727,33,,S
722,0,3,"Jensen, Mr. Svend Lauritz",male,17,1,0,350048,7.0542,,S
723,0,2,"Gillespie, Mr. William Henry",male,34,0,0,12233,13,,S
724,0,2,"Hodges, Mr. Henry Price",male,50,0,0,250643,13,,S
725,1,1,"Chambers, Mr. Norman Campbell",male,27,1,0,113806,53.1,E8,S
726,0,3,"Oreskovic, Mr. Luka",male,20,0,0,315094,8.6625,,S
727,1,2,"Renouf, Mrs. Peter Henry (Lillian Jefferys)",female,30,3,0,31027,21,,S
728,1,3,"Mannion, Miss. Margareth",female,,0,0,36866,7.7375,,Q
729,0,2,"Bryhl, Mr. Kurt Arnold Gottfrid",male,25,1,0,236853,26,,S
730,0,3,"Ilmakangas, Miss. Pieta Sofia",female,25,1,0,STON/O2. 3101271,7.925,,S
731,1,1,"Allen, Miss. Elisabeth Walton",female,29,0,0,24160,211.3375,B5,S
732,0,3,"Hassan, Mr. Houssein G N",male,11,0,0,2699,18.7875,,C
733,0,2,"Knight, Mr. Robert J",male,,0,0,239855,0,,S
734,0,2,"Berriman, Mr. William John",male,23,0,0,28425,13,,S
735,0,2,"Troupiansky, Mr. Moses Aaron",male,23,0,0,233639,13,,S
736,0,3,"Williams, Mr. Leslie",male,28.5,0,0,54636,16.1,,S
737,0,3,"Ford, Mrs. Edward (Margaret Ann Watson)",female,48,1,3,W./C. 6608,34.375,,S
738,1,1,"Lesurer, Mr. Gustave J",male,35,0,0,PC 17755,512.3292,B101,C
739,0,3,"Ivanoff, Mr. Kanio",male,,0,0,349201,7.8958,,S
740,0,3,"Nankoff, Mr. Minko",male,,0,0,349218,7.8958,,S
741,1,1,"Hawksford, Mr. Walter James",male,,0,0,16988,30,D45,S
742,0,1,"Cavendish, Mr. Tyrell William",male,36,1,0,19877,78.85,C46,S
743,1,1,"Ryerson, Miss. Susan Parker ""Suzette""",female,21,2,2,PC 17608,262.375,B57 B59 B63 B66,C
744,0,3,"McNamee, Mr. Neal",male,24,1,0,376566,16.1,,S
745,1,3,"Stranden, Mr. Juho",male,31,0,0,STON/O 2. 3101288,7.925,,S
746,0,1,"Crosby, Capt. Edward Gifford",male,70,1,1,WE/P 5735,71,B22,S
747,0,3,"Abbott, Mr. Rossmore Edward",male,16,1,1,C.A. 2673,20.25,,S
748,1,2,"Sinkkonen, Miss. Anna",female,30,0,0,250648,13,,S
749,0,1,"Marvin, Mr. Daniel Warner",male,19,1,0,113773,53.1,D30,S
750,0,3,"Connaghton, Mr. Michael",male,31,0,0,335097,7.75,,Q
751,1,2,"Wells, Miss. Joan",female,4,1,1,29103,23,,S
752,1,3,"Moor, Master. Meier",male,6,0,1,392096,12.475,E121,S
753,0,3,"Vande Velde, Mr. Johannes Joseph",male,33,0,0,345780,9.5,,S
754,0,3,"Jonkoff, Mr. Lalio",male,23,0,0,349204,7.8958,,S
755,1,2,"Herman, Mrs. Samuel (Jane Laver)",female,48,1,2,220845,65,,S
756,1,2,"Hamalainen, Master. Viljo",male,0.67,1,1,250649,14.5,,S
757,0,3,"Carlsson, Mr. August Sigfrid",male,28,0,0,350042,7.7958,,S
758,0,2,"Bailey, Mr. Percy Andrew",male,18,0,0,29108,11.5,,S
759,0,3,"Theobald, Mr. Thomas Leonard",male,34,0,0,363294,8.05,,S
760,1,1,"Rothes, the Countess. of (Lucy Noel Martha Dyer-Edwards)",female,33,0,0,110152,86.5,B77,S
761,0,3,"Garfirth, Mr. John",male,,0,0,358585,14.5,,S
762,0,3,"Nirva, Mr. Iisakki Antino Aijo",male,41,0,0,SOTON/O2 3101272,7.125,,S
763,1,3,"Barah, Mr. Hanna Assi",male,20,0,0,2663,7.2292,,C
764,1,1,"Carter, Mrs. William Ernest (Lucile Polk)",female,36,1,2,113760,120,B96 B98,S
765,0,3,"Eklund, Mr. Hans Linus",male,16,0,0,347074,7.775,,S
766,1,1,"Hogeboom, Mrs. John C (Anna Andrews)",female,51,1,0,13502,77.9583,D11,S
767,0,1,"Brewe, Dr. Arthur Jackson",male,,0,0,112379,39.6,,C
768,0,3,"Mangan, Miss. Mary",female,30.5,0,0,364850,7.75,,Q
769,0,3,"Moran, Mr. Daniel J",male,,1,0,371110,24.15,,Q
770,0,3,"Gronnestad, Mr. Daniel Danielsen",male,32,0,0,8471,8.3625,,S
771,0,3,"Lievens, Mr. Rene Aime",male,24,0,0,345781,9.5,,S
772,0,3,"Jensen, Mr. Niels Peder",male,48,0,0,350047,7.8542,,S
773,0,2,"Mack, Mrs. (Mary)",female,57,0,0,S.O./P.P. 3,10.5,E77,S
774,0,3,"Elias, Mr. Dibo",male,,0,0,2674,7.225,,C
775,1,2,"Hocking, Mrs. Elizabeth (Eliza Needs)",female,54,1,3,29105,23,,S
776,0,3,"Myhrman, Mr. Pehr Fabian Oliver Malkolm",male,18,0,0,347078,7.75,,S
777,0,3,"Tobin, Mr. Roger",male,,0,0,383121,7.75,F38,Q
778,1,3,"Emanuel, Miss. Virginia Ethel",female,5,0,0,364516,12.475,,S
779,0,3,"Kilgannon, Mr. Thomas J",male,,0,0,36865,7.7375,,Q
780,1,1,"Robert, Mrs. Edward Scott (Elisabeth Walton McMillan)",female,43,0,1,24160,211.3375,B3,S
781,1,3,"Ayoub, Miss. Banoura",female,13,0,0,2687,7.2292,,C
782,1,1,"Dick, Mrs. Albert Adrian (Vera Gillespie)",female,17,1,0,17474,57,B20,S
783,0,1,"Long, Mr. Milton Clyde",male,29,0,0,113501,30,D6,S
784,0,3,"Johnston, Mr. Andrew G",male,,1,2,W./C. 6607,23.45,,S
785,0,3,"Ali, Mr. William",male,25,0,0,SOTON/O.Q. 3101312,7.05,,S
786,0,3,"Harmer, Mr. Abraham (David Lishin)",male,25,0,0,374887,7.25,,S
787,1,3,"Sjoblom, Miss. Anna Sofia",female,18,0,0,3101265,7.4958,,S
788,0,3,"Rice, Master. George Hugh",male,8,4,1,382652,29.125,,Q
789,1,3,"Dean, Master. Bertram Vere",male,1,1,2,C.A. 2315,20.575,,S
790,0,1,"Guggenheim, Mr. Benjamin",male,46,0,0,PC 17593,79.2,B82 B84,C
791,0,3,"Keane, Mr. Andrew ""Andy""",male,,0,0,12460,7.75,,Q
792,0,2,"Gaskell, Mr. Alfred",male,16,0,0,239865,26,,S
793,0,3,"Sage, Miss. Stella Anna",female,,8,2,CA. 2343,69.55,,S
794,0,1,"Hoyt, Mr. William Fisher",male,,0,0,PC 17600,30.6958,,C
795,0,3,"Dantcheff, Mr. Ristiu",male,25,0,0,349203,7.8958,,S
796,0,2,"Otter, Mr. Richard",male,39,0,0,28213,13,,S
797,1,1,"Leader, Dr. Alice (Farnham)",female,49,0,0,17465,25.9292,D17,S
798,1,3,"Osman, Mrs. Mara",female,31,0,0,349244,8.6833,,S
799,0,3,"Ibrahim Shawah, Mr. Yousseff",male,30,0,0,2685,7.2292,,C
800,0,3,"Van Impe, Mrs. Jean Baptiste (Rosalie Paula Govaert)",female,30,1,1,345773,24.15,,S
801,0,2,"Ponesell, Mr. Martin",male,34,0,0,250647,13,,S
802,1,2,"Collyer, Mrs. Harvey (Charlotte Annie Tate)",female,31,1,1,C.A. 31921,26.25,,S
803,1,1,"Carter, Master. William Thornton II",male,11,1,2,113760,120,B96 B98,S
804,1,3,"Thomas, Master. Assad Alexander",male,0.42,0,1,2625,8.5167,,C
805,1,3,"Hedman, Mr. Oskar Arvid",male,27,0,0,347089,6.975,,S
806,0,3,"Johansson, Mr. Karl Johan",male,31,0,0,347063,7.775,,S
807,0,1,"Andrews, Mr. Thomas Jr",male,39,0,0,112050,0,A36,S
808,0,3,"Pettersson, Miss. Ellen Natalia",female,18,0,0,347087,7.775,,S
809,0,2,"Meyer, Mr. August",male,39,0,0,248723,13,,S
810,1,1,"Chambers, Mrs. Norman Campbell (Bertha Griggs)",female,33,1,0,113806,53.1,E8,S
811,0,3,"Alexander, Mr. William",male,26,0,0,3474,7.8875,,S
812,0,3,"Lester, Mr. James",male,39,0,0,A/4 48871,24.15,,S
813,0,2,"Slemen, Mr. Richard James",male,35,0,0,28206,10.5,,S
814,0,3,"Andersson, Miss. Ebba Iris Alfrida",female,6,4,2,347082,31.275,,S
815,0,3,"Tomlin, Mr. Ernest Portage",male,30.5,0,0,364499,8.05,,S
816,0,1,"Fry, Mr. Richard",male,,0,0,112058,0,B102,S
817,0,3,"Heininen, Miss. Wendla Maria",female,23,0,0,STON/O2. 3101290,7.925,,S
818,0,2,"Mallet, Mr. Albert",male,31,1,1,S.C./PARIS 2079,37.0042,,C
819,0,3,"Holm, Mr. John Fredrik Alexander",male,43,0,0,C 7075,6.45,,S
820,0,3,"Skoog, Master. Karl Thorsten",male,10,3,2,347088,27.9,,S
821,1,1,"Hays, Mrs. Charles Melville (Clara Jennings Gregg)",female,52,1,1,12749,93.5,B69,S
822,1,3,"Lulic, Mr. Nikola",male,27,0,0,315098,8.6625,,S
823,0,1,"Reuchlin, Jonkheer. John George",male,38,0,0,19972,0,,S
824,1,3,"Moor, Mrs. (Beila)",female,27,0,1,392096,12.475,E121,S
825,0,3,"Panula, Master. Urho Abraham",male,2,4,1,3101295,39.6875,,S
826,0,3,"Flynn, Mr. John",male,,0,0,368323,6.95,,Q
827,0,3,"Lam, Mr. Len",male,,0,0,1601,56.4958,,S
828,1,2,"Mallet, Master. Andre",male,1,0,2,S.C./PARIS 2079,37.0042,,C
829,1,3,"McCormack, Mr. Thomas Joseph",male,,0,0,367228,7.75,,Q
830,1,1,"Stone, Mrs. George Nelson (Martha Evelyn)",female,62,0,0,113572,80,B28,
831,1,3,"Yasbeck, Mrs. Antoni (Selini Alexander)",female,15,1,0,2659,14.4542,,C
832,1,2,"Richards, Master. George Sibley",male,0.83,1,1,29106,18.75,,S
833,0,3,"Saad, Mr. Amin",male,,0,0,2671,7.2292,,C
834,0,3,"Augustsson, Mr. Albert",male,23,0,0,347468,7.8542,,S
835,0,3,"Allum, Mr. Owen George",male,18,0,0,2223,8.3,,S
836,1,1,"Compton, Miss. Sara Rebecca",female,39,1,1,PC 17756,83.1583,E49,C
837,0,3,"Pasic, Mr. Jakob",male,21,0,0,315097,8.6625,,S
838,0,3,"Sirota, Mr. Maurice",male,,0,0,392092,8.05,,S
839,1,3,"Chip, Mr. Chang",male,32,0,0,1601,56.4958,,S
840,1,1,"Marechal, Mr. Pierre",male,,0,0,11774,29.7,C47,C
841,0,3,"Alhomaki, Mr. Ilmari Rudolf",male,20,0,0,SOTON/O2 3101287,7.925,,S
842,0,2,"Mudd, Mr. Thomas Charles",male,16,0,0,S.O./P.P. 3,10.5,,S
843,1,1,"Serepeca, Miss. Augusta",female,30,0,0,113798,31,,C
844,0,3,"Lemberopolous, Mr. Peter L",male,34.5,0,0,2683,6.4375,,C
845,0,3,"Culumovic, Mr. Jeso",male,17,0,0,315090,8.6625,,S
846,0,3,"Abbing, Mr. Anthony",male,42,0,0,C.A. 5547,7.55,,S
847,0,3,"Sage, Mr. Douglas Bullen",male,,8,2,CA. 2343,69.55,,S
848,0,3,"Markoff, Mr. Marin",male,35,0,0,349213,7.8958,,C
849,0,2,"Harper, Rev. John",male,28,0,1,248727,33,,S
850,1,1,"Goldenberg, Mrs. Samuel L (Edwiga Grabowska)",female,,1,0,17453,89.1042,C92,C
851,0,3,"Andersson, Master. Sigvard Harald Elias",male,4,4,2,347082,31.275,,S
852,0,3,"Svensson, Mr. Johan",male,74,0,0,347060,7.775,,S
853,0,3,"Boulos, Miss. Nourelain",female,9,1,1,2678,15.2458,,C
854,1,1,"Lines, Miss. Mary Conover",female,16,0,1,PC 17592,39.4,D28,S
855,0,2,"Carter, Mrs. Ernest Courtenay (Lilian Hughes)",female,44,1,0,244252,26,,S
856,1,3,"Aks, Mrs. Sam (Leah Rosen)",female,18,0,1,392091,9.35,,S
857,1,1,"Wick, Mrs. George Dennick (Mary Hitchcock)",female,45,1,1,36928,164.8667,,S
858,1,1,"Daly, Mr. Peter Denis ",male,51,0,0,113055,26.55,E17,S
859,1,3,"Baclini, Mrs. Solomon (Latifa Qurban)",female,24,0,3,2666,19.2583,,C
860,0,3,"Razi, Mr. Raihed",male,,0,0,2629,7.2292,,C
861,0,3,"Hansen, Mr. Claus Peter",male,41,2,0,350026,14.1083,,S
862,0,2,"Giles, Mr. Frederick Edward",male,21,1,0,28134,11.5,,S
863,1,1,"Swift, Mrs. Frederick Joel (Margaret Welles Barron)",female,48,0,0,17466,25.9292,D17,S
864,0,3,"Sage, Miss. Dorothy Edith ""Dolly""",female,,8,2,CA. 2343,69.55,,S
865,0,2,"Gill, Mr. John William",male,24,0,0,233866,13,,S
866,1,2,"Bystrom, Mrs. (Karolina)",female,42,0,0,236852,13,,S
867,1,2,"Duran y More, Miss. Asuncion",female,27,1,0,SC/PARIS 2149,13.8583,,C
868,0,1,"Roebling, Mr. Washington Augustus II",male,31,0,0,PC 17590,50.4958,A24,S
869,0,3,"van Melkebeke, Mr. Philemon",male,,0,0,345777,9.5,,S
870,1,3,"Johnson, Master. Harold Theodor",male,4,1,1,347742,11.1333,,S
871,0,3,"Balkic, Mr. Cerin",male,26,0,0,349248,7.8958,,S
872,1,1,"Beckwith, Mrs. Richard Leonard (Sallie Monypeny)",female,47,1,1,11751,52.5542,D35,S
873,0,1,"Carlsson, Mr. Frans Olof",male,33,0,0,695,5,B51 B53 B55,S
874,0,3,"Vander Cruyssen, Mr. Victor",male,47,0,0,345765,9,,S
875,1,2,"Abelson, Mrs. Samuel (Hannah Wizosky)",female,28,1,0,P/PP 3381,24,,C
876,1,3,"Najib, Miss. Adele Kiamie ""Jane""",female,15,0,0,2667,7.225,,C
877,0,3,"Gustafsson, Mr. Alfred Ossian",male,20,0,0,7534,9.8458,,S
878,0,3,"Petroff, Mr. Nedelio",male,19,0,0,349212,7.8958,,S
879,0,3,"Laleff, Mr. Kristo",male,,0,0,349217,7.8958,,S
880,1,1,"Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)",female,56,0,1,11767,83.1583,C50,C
881,1,2,"Shelley, Mrs. William (Imanita Parrish Hall)",female,25,0,1,230433,26,,S
882,0,3,"Markun, Mr. Johann",male,33,0,0,349257,7.8958,,S
883,0,3,"Dahlberg, Miss. Gerda Ulrika",female,22,0,0,7552,10.5167,,S
884,0,2,"Banfield, Mr. Frederick James",male,28,0,0,C.A./SOTON 34068,10.5,,S
885,0,3,"Sutehall, Mr. Henry Jr",male,25,0,0,SOTON/OQ 392076,7.05,,S
886,0,3,"Rice, Mrs. William (Margaret Norton)",female,39,0,5,382652,29.125,,Q
887,0,2,"Montvila, Rev. Juozas",male,27,0,0,211536,13,,S
888,1,1,"Graham, Miss. Margaret Edith",female,19,0,0,112053,30,B42,S
889,0,3,"Johnston, Miss. Catherine Helen ""Carrie""",female,,1,2,W./C. 6607,23.45,,S
890,1,1,"Behr, Mr. Karl Howell",male,26,0,0,111369,30,C148,C
891,0,3,"Dooley, Mr. Patrick",male,32,0,0,370376,7.75,,Q
                                                                                                                                                                                                                                                      ./titanic_survival_exploration.ipynb                                                                0000644 0000000 0000000 00000227446 13312573030 017002  0                                                                                                    ustar   root                            root                                                                                                                                                                                                                   {
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Machine Learning Engineer Nanodegree\n",
    "## Introduction and Foundations\n",
    "## Project: Titanic Survival Exploration\n",
    "\n",
    "In 1912, the ship RMS Titanic struck an iceberg on its maiden voyage and sank, resulting in the deaths of most of its passengers and crew. In this introductory project, we will explore a subset of the RMS Titanic passenger manifest to determine which features best predict whether someone survived or did not survive. To complete this project, you will need to implement several conditional predictions and answer the questions below. Your project submission will be evaluated based on the completion of the code and your responses to the questions.\n",
    "> **Tip:** Quoted sections like this will provide helpful instructions on how to navigate and use an iPython notebook. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Getting Started\n",
    "To begin working with the RMS Titanic passenger data, we'll first need to `import` the functionality we need, and load our data into a `pandas` DataFrame.  \n",
    "Run the code cell below to load our data and display the first few entries (passengers) for examination using the `.head()` function.\n",
    "> **Tip:** You can run a code cell by clicking on the cell and using the keyboard shortcut **Shift + Enter** or **Shift + Return**. Alternatively, a code cell can be executed using the **Play** button in the hotbar after selecting it. Markdown cells (text cells like this one) can be edited by double-clicking, and saved using these same shortcuts. [Markdown](http://daringfireball.net/projects/markdown/syntax) allows you to write easy-to-read plain text that can be converted to HTML."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style>\n",
       "    .dataframe thead tr:only-child th {\n",
       "        text-align: right;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: left;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>PassengerId</th>\n",
       "      <th>Survived</th>\n",
       "      <th>Pclass</th>\n",
       "      <th>Name</th>\n",
       "      <th>Sex</th>\n",
       "      <th>Age</th>\n",
       "      <th>SibSp</th>\n",
       "      <th>Parch</th>\n",
       "      <th>Ticket</th>\n",
       "      <th>Fare</th>\n",
       "      <th>Cabin</th>\n",
       "      <th>Embarked</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>3</td>\n",
       "      <td>Braund, Mr. Owen Harris</td>\n",
       "      <td>male</td>\n",
       "      <td>22.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>A/5 21171</td>\n",
       "      <td>7.2500</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>\n",
       "      <td>female</td>\n",
       "      <td>38.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>PC 17599</td>\n",
       "      <td>71.2833</td>\n",
       "      <td>C85</td>\n",
       "      <td>C</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>3</td>\n",
       "      <td>Heikkinen, Miss. Laina</td>\n",
       "      <td>female</td>\n",
       "      <td>26.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>STON/O2. 3101282</td>\n",
       "      <td>7.9250</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>\n",
       "      <td>female</td>\n",
       "      <td>35.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>113803</td>\n",
       "      <td>53.1000</td>\n",
       "      <td>C123</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>0</td>\n",
       "      <td>3</td>\n",
       "      <td>Allen, Mr. William Henry</td>\n",
       "      <td>male</td>\n",
       "      <td>35.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>373450</td>\n",
       "      <td>8.0500</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   PassengerId  Survived  Pclass  \\\n",
       "0            1         0       3   \n",
       "1            2         1       1   \n",
       "2            3         1       3   \n",
       "3            4         1       1   \n",
       "4            5         0       3   \n",
       "\n",
       "                                                Name     Sex   Age  SibSp  \\\n",
       "0                            Braund, Mr. Owen Harris    male  22.0      1   \n",
       "1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  38.0      1   \n",
       "2                             Heikkinen, Miss. Laina  female  26.0      0   \n",
       "3       Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  35.0      1   \n",
       "4                           Allen, Mr. William Henry    male  35.0      0   \n",
       "\n",
       "   Parch            Ticket     Fare Cabin Embarked  \n",
       "0      0         A/5 21171   7.2500   NaN        S  \n",
       "1      0          PC 17599  71.2833   C85        C  \n",
       "2      0  STON/O2. 3101282   7.9250   NaN        S  \n",
       "3      0            113803  53.1000  C123        S  \n",
       "4      0            373450   8.0500   NaN        S  "
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Import libraries necessary for this project\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "from IPython.display import display # Allows the use of display() for DataFrames\n",
    "\n",
    "# Import supplementary visualizations code visuals.py\n",
    "import visuals as vs\n",
    "\n",
    "# Pretty display for notebooks\n",
    "%matplotlib inline\n",
    "\n",
    "# Load the dataset\n",
    "in_file = 'titanic_data.csv'\n",
    "full_data = pd.read_csv(in_file)\n",
    "\n",
    "# Print the first few entries of the RMS Titanic data\n",
    "display(full_data.head())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "From a sample of the RMS Titanic data, we can see the various features present for each passenger on the ship:\n",
    "- **Survived**: Outcome of survival (0 = No; 1 = Yes)\n",
    "- **Pclass**: Socio-economic class (1 = Upper class; 2 = Middle class; 3 = Lower class)\n",
    "- **Name**: Name of passenger\n",
    "- **Sex**: Sex of the passenger\n",
    "- **Age**: Age of the passenger (Some entries contain `NaN`)\n",
    "- **SibSp**: Number of siblings and spouses of the passenger aboard\n",
    "- **Parch**: Number of parents and children of the passenger aboard\n",
    "- **Ticket**: Ticket number of the passenger\n",
    "- **Fare**: Fare paid by the passenger\n",
    "- **Cabin** Cabin number of the passenger (Some entries contain `NaN`)\n",
    "- **Embarked**: Port of embarkation of the passenger (C = Cherbourg; Q = Queenstown; S = Southampton)\n",
    "\n",
    "Since we're interested in the outcome of survival for each passenger or crew member, we can remove the **Survived** feature from this dataset and store it as its own separate variable `outcomes`. We will use these outcomes as our prediction targets.  \n",
    "Run the code cell below to remove **Survived** as a feature of the dataset and store it in `outcomes`."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style>\n",
       "    .dataframe thead tr:only-child th {\n",
       "        text-align: right;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: left;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>PassengerId</th>\n",
       "      <th>Pclass</th>\n",
       "      <th>Name</th>\n",
       "      <th>Sex</th>\n",
       "      <th>Age</th>\n",
       "      <th>SibSp</th>\n",
       "      <th>Parch</th>\n",
       "      <th>Ticket</th>\n",
       "      <th>Fare</th>\n",
       "      <th>Cabin</th>\n",
       "      <th>Embarked</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>3</td>\n",
       "      <td>Braund, Mr. Owen Harris</td>\n",
       "      <td>male</td>\n",
       "      <td>22.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>A/5 21171</td>\n",
       "      <td>7.2500</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>1</td>\n",
       "      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>\n",
       "      <td>female</td>\n",
       "      <td>38.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>PC 17599</td>\n",
       "      <td>71.2833</td>\n",
       "      <td>C85</td>\n",
       "      <td>C</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>Heikkinen, Miss. Laina</td>\n",
       "      <td>female</td>\n",
       "      <td>26.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>STON/O2. 3101282</td>\n",
       "      <td>7.9250</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>\n",
       "      <td>female</td>\n",
       "      <td>35.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>113803</td>\n",
       "      <td>53.1000</td>\n",
       "      <td>C123</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>3</td>\n",
       "      <td>Allen, Mr. William Henry</td>\n",
       "      <td>male</td>\n",
       "      <td>35.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>373450</td>\n",
       "      <td>8.0500</td>\n",
       "      <td>NaN</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   PassengerId  Pclass                                               Name  \\\n",
       "0            1       3                            Braund, Mr. Owen Harris   \n",
       "1            2       1  Cumings, Mrs. John Bradley (Florence Briggs Th...   \n",
       "2            3       3                             Heikkinen, Miss. Laina   \n",
       "3            4       1       Futrelle, Mrs. Jacques Heath (Lily May Peel)   \n",
       "4            5       3                           Allen, Mr. William Henry   \n",
       "\n",
       "      Sex   Age  SibSp  Parch            Ticket     Fare Cabin Embarked  \n",
       "0    male  22.0      1      0         A/5 21171   7.2500   NaN        S  \n",
       "1  female  38.0      1      0          PC 17599  71.2833   C85        C  \n",
       "2  female  26.0      0      0  STON/O2. 3101282   7.9250   NaN        S  \n",
       "3  female  35.0      1      0            113803  53.1000  C123        S  \n",
       "4    male  35.0      0      0            373450   8.0500   NaN        S  "
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Store the 'Survived' feature in a new variable and remove it from the dataset\n",
    "outcomes = full_data['Survived']\n",
    "data = full_data.drop('Survived', axis = 1)\n",
    "\n",
    "# Show the new dataset with 'Survived' removed\n",
    "display(data.head())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The very same sample of the RMS Titanic data now shows the **Survived** feature removed from the DataFrame. Note that `data` (the passenger data) and `outcomes` (the outcomes of survival) are now *paired*. That means for any passenger `data.loc[i]`, they have the survival outcome `outcomes[i]`.\n",
    "\n",
    "To measure the performance of our predictions, we need a metric to score our predictions against the true outcomes of survival. Since we are interested in how *accurate* our predictions are, we will calculate the proportion of passengers where our prediction of their survival is correct. Run the code cell below to create our `accuracy_score` function and test a prediction on the first five passengers.  \n",
    "\n",
    "**Think:** *Out of the first five passengers, if we predict that all of them survived, what would you expect the accuracy of our predictions to be?*"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Predictions have an accuracy of 60.00%.\n"
     ]
    }
   ],
   "source": [
    "def accuracy_score(truth, pred):\n",
    "    \"\"\" Returns accuracy score for input truth and predictions. \"\"\"\n",
    "    # Ensure that the number of predictions matches number of outcomes\n",
    "    if len(truth) == len(pred): \n",
    "        \n",
    "        # Calculate and return the accuracy as a percent\n",
    "        return \"Predictions have an accuracy of {:.2f}%.\".format((truth == pred).mean()*100)\n",
    "    \n",
    "    else:\n",
    "        return \"Number of predictions does not match number of outcomes!\"\n",
    "    \n",
    "# Test the 'accuracy_score' function\n",
    "predictions = pd.Series(np.ones(5, dtype = int))\n",
    "print(accuracy_score(outcomes[:5], predictions))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "> **Tip:** If you save an iPython Notebook, the output from running code blocks will also be saved. However, the state of your workspace will be reset once a new session is started. Make sure that you run all of the code blocks from your previous session to reestablish variables and functions before picking up where you last left off.\n",
    "\n",
    "# Making Predictions\n",
    "\n",
    "If we were asked to make a prediction about any passenger aboard the RMS Titanic whom we knew nothing about, then the best prediction we could make would be that they did not survive. This is because we can assume that a majority of the passengers (more than 50%) did not survive the ship sinking.  \n",
    "The `predictions_0` function below will always predict that a passenger did not survive."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "def predictions_0(data):\n",
    "    \"\"\" Model with no features. Always predicts a passenger did not survive. \"\"\"\n",
    "\n",
    "    predictions = []\n",
    "    for _, passenger in data.iterrows():\n",
    "        \n",
    "        # Predict the survival of 'passenger'\n",
    "        predictions.append(0)\n",
    "    \n",
    "    # Return our predictions\n",
    "    return pd.Series(predictions)\n",
    "\n",
    "# Make the predictions\n",
    "predictions = predictions_0(data)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Question 1\n",
    "\n",
    "* Using the RMS Titanic data, how accurate would a prediction be that none of the passengers survived?\n",
    "\n",
    "**Hint:** Run the code cell below to see the accuracy of this prediction."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Predictions have an accuracy of 61.62%.\n"
     ]
    }
   ],
   "source": [
    "print(accuracy_score(outcomes, predictions))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Answer:** *Replace this text with the prediction accuracy you found above.*"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "***\n",
    "Let's take a look at whether the feature **Sex** has any indication of survival rates among passengers using the `survival_stats` function. This function is defined in the `visuals.py` Python script included with this project. The first two parameters passed to the function are the RMS Titanic data and passenger survival outcomes, respectively. The third parameter indicates which feature we want to plot survival statistics across.  \n",
    "Run the code cell below to plot the survival outcomes of passengers based on their sex."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAfgAAAGDCAYAAADHzQJ9AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3XmYXVWZ7/HvSyUQhEgYgg0ECCLajAmQMAiENNAMEgIqkCAyKFdAROiLrYKKTA4oYjeoiNDYpAUNEFsI0wUbDYhMEkhQCHYYlEQiGSAhhDHw3j/2rnBSqeEkVaeqsvP9PE89dfa09jrj76y119k7MhNJklQtq/V0BSRJUtcz4CVJqiADXpKkCjLgJUmqIANekqQKMuAlSaogA17qRhFxdETc2QXlHB8R93ZFnVZw/5dHxNkrsN1mEfFKRDQ1ol5dsf+IyIj4QHfWS2oEA34VFhF/iYjXyg+8FyLiPyNi7Z6uV3eLiEER8cuImBsRCyLijxFxfCP2lZnXZub+jSi7VkScEBFPRsTC8rm9NSL6l8uujohvLEdZy3yZyMyTM/OCOrb9S0TsV7Pdc5m5dma+vTz3p53yfxIRl9VM942IRW3M263l/iNiUkT8n07s/9yIOLdm+isR8Wz5npoZEdetaNk1ZY6MiEltLBtcfiF5peZvahfs89yIuKaz5ahnGfA6JDPXBnYChgNf6+H6NFRE9Gll9s+AGcDmwPrAscALXVh+t4qIvYFvAUdlZn9ga+D6nq1Vw9wD7F0zPQx4DhjRYh7A5EZWJCKOA44B9ivfU8OAuxq5zxoDyi8ua2fmkG7aZ5t6w/tABrxKmfk34HZgO4CI+FRETCtbgM9ExEnN60bEBhFxS0TMj4gXI+J3EbFauezLEfG3crs/R8S+5fzVIuLMiHg6IuZFxPURsV65rLkVclxEPFe2pL9as781I2JcRLxU1ulLETGzZvnGZQt8Ttl6Oq1m2bkRMSEiromIl4HjW7n7w4GrM3NRZi7OzEcz8/Zy+5G1+yrnLWmVtlL+V8pekfVq1t+xvE99a1vDZTf391qUfVNEnFHebn68FkbEExHx0TqfzuHA/Zn5KEBmvpiZ4zJzYUScCBwNfKls7d3c3r4iYmvgcmD3cv355fwlvQBtvR4i4mfAZsDN5bZfqnmu+5TbrhdFz9Hz5fN7Y3tltnJf7wa2jogNyum9gPHAWi3m3Z+Zb9XuPyK+WS77YVm/H9aUu19ETC/r9KOIiDof9zsy8+nycf97Zl7RvDAi1omIqyJiVvke+UaUhwoi4scRMaFm3e9ExF117rdNEfHp8j3zUkTcERGb1yy7JCJmRMTLETE5IvYq5x8IfAUYEzU9AtGiNyZqWvk1j+sJEfEc8Jty/m4RcV/5PE6NiJGduT9aTpnp3yr6B/yForUBsCnwOHBBOX0wsCUQFC2kV4GdymXfpvjQ71v+7VWu9yGKlvDG5XqDgS3L2/8CPAAMAtYAfgL8oma9BK4E1gSGAG8AW5fLL6T4IF+33P4xYGa5bDWKltnXgdWB9wPPAAeUy88F3gIOK9dds5XH4X+A3wNjgc1aLBvZvK82Hrdlyqf4cPtMzfoXAZeXt48H7i1vjygfryin1wVeq3n8jgA2LssdAywCNmpZTiv3Z6+ynPOAPYA1Wiy/GvhGi3nLta/aMtp6PbR8rFo8133K6VuB68r73hfYu6MyW7m/zwIfLW/fAuwDXNti3tfb2P8k4P+0KC/LbQZQfEGZAxxYx/vpk8CLwBcpWu9NLZbfSPG6XwvYEHgIOKlc9h7gf8vHei9gLjCojn0udX9aLDsMeIqiB6cPRe/cfS3qu3657AvA34F+Na/ra9p63bdcp6Ye/1XevzWBTYB5wEcoXlf/XE4P7OnPvlXlzxa8bixbZfdShOi3ADLz1sx8Ogt3A3dSfPBAEWgbAZtn5luZ+bss3uVvU4T3NhHRNzP/kmVrBjgJ+GpmzszMNyg+HA6PpbvyzsvM1zJzKjCVIugBjgS+lZkvZeZM4NKabYZTfGCcn5lvZuYzFF8Uxtasc39m3piZ72Tma608BkcAvwPOBp6NiCkRMXw5HsOW5f8cOAqgbIGNLee19DuKD8Xmx/XwsqznATLzhsx8viz3OmA6sEtHlcnM3wEfozjsciswLyK+H+0MLFvRfZXaej20KyI2Ag4CTi6f27fK19rylnk3MKJs4e9C8UXydzXz9ijXWR4XZub8zHwO+C0wtKMNMvMa4PPAAeX+ZkfEmeV9fV95X/8li56i2cC/Ub5OM/NVisD9PnAN8PnytV6vuWUreX5E/Gs57yTg25k5LTMXU7y3hza34jPzmsycl0Wv1cUU790PLcc+W3Nuef9eK+/PbZl5W/m6+jXwMEXgqxsY8DosMwdk5uaZeUpzAEbEQRHxQNk9Op/iTdnc5XkRRcvgzii6788EyMynKFrq51J8uI2PiI3LbTYHftX8IQRMo/hC8L6auvy95varQPOAv40pWrrNam9vDmxc8+E2n6J78X1trL+MMlzOzMxty+2mUHzxqbd7tGX5Eyi6tDemaKUnReC03G9SdCcfVc76BEXLE4CIOLb8stF8v7bj3eegXZl5e2YeAqwHHErRMmxzMFln9kUbr4c6bAq8mJkvdbLMeyge5+2BZ8qwvLdm3prAg3XWqVlbr8V2ZTGIcj+K1v/JwPkRcQDF67QvMKvmMf4JRUu+eduHKHqfguUfM7FB+T4ekJnNh302By6p2d+LZdmbAETEF8ru+wXl8nWo/zlvS8v35hEt3pt7UnxxUzcw4LWMiFgD+CXwPeB9mTkAuI3iw4HMXJiZX8jM9wOHAGdEeaw9M3+emXtSvLkT+E5Z7AzgoJoPoQGZ2S+LY/8dmUXRNd9s05rbM4BnW5TbPzNrWwl1XzIxM+eW93tjinBcRNF9CkDZCh7YcrMWZcyn6PE4kiK0f9FO6/MXFD0ZmwO7UjzulNNXAqcC65fPwZ8on4PluD/vZOZdFIcNtmutvnXsq93Hr73XQwfbzgDWi4gBy1lmS/dQ9PYczLtfpB6neJ0cDPwhM19vq/rt3bcVVfY63EBxOGk7ivv6BksH8XvLL5UARMTnKFrRzwNf6oJqzKA4BFD73lgzM+8rj7d/meI1um75nC+g/ed8qfcC8A+trFO73QzgZy32v1ZmXtjpe6a6GPBqzeoUHzRzgMURcRCw5KddETEqIj5QtnBfpmiJvx0RH4qIfcovCK9THAdu/jnU5cA3m7sHI2JgRBxaZ32uB86KiHUjYhOKIGr2EPByFIP71oyIpojYbnm62MsBTdtFMfCqP/BZ4KnMnEdxXLRfRBwcEX0pjmOuUUexP6cYjf9xWu+eByCLgXBzgP+gGKA1v1y0FsWH5Zyyjp/i3YDu6P4cGhFjy8crImIXinEUD5SrvEAxVqFZR/t6ARgUEau3sb9WXw9t7Kv2vs+iGNh5WVnXvhExoo4yW5bzVLmf0ykDvvxC9WA5757WtuuofssrigGUB0dE/ygGGR4EbAs8WN7XO4GLI+K95fIto/jFAxHxQeAbFN3ax1AMguzwsEAHLqd432xb7mOdiDiiXNYfWEzxnPeJiK8D763Z9gVgcCw9sHEKMLZ8noZRHFJqzzXAIRFxQPm+7BfFoNVBHWynLmLAaxmZuRA4jSJYX6JohU6sWWUrioFprwD3A5dl5iSK4LuQYoDQ3ym6H79SbnNJWcadEbGQImx2rbNK5wMzKQZT/Q9FF/gbZV3fpmjhDS2Xz6UIy3WW4y6/B/gVMJ+ii3RzYHRZ/gLglLLMv1G0Yuo5NjqR4nF6IYsxBe35BbAfNV8EMvMJ4GKKx/cFiq7m39d5f14CPkNxHP1lig/aizKzufv/KopxEvMj4sY69vUbihbx3yNibiv7a+v1AMVgua+1ODZc6xiK4+1PArMpDvF0VGZr7qHoWamt9+8oXoPtBfwlFD0oL0XEpe2sV4+XKV7vz1G8lr4LfDYzm88hcCzFl+cnKJ6jCcBGUYxDuQb4TmZOzczpZTk/K78sr5DM/BVFD9r4KH7h8SeKcQAAd1B8ufpf4K8UX8hru9dvKP/Pi4hHyttnUwy8fYliAGebX1zL/c+gODz0FYovEjMoBiCaO92keaSrtNKIiM8CYzNz7w5XlqRVlN+k1OtFxEYRsUfZrfkhip/0/Kqn6yVJvZlnG9LKYHWKEcdbUHR9jgcua3cLSVrF2UUvSVIF2UUvSVIFGfCSJFXQSn0MfoMNNsjBgwf3dDUkSeoWkydPnpuZLU+21aqVOuAHDx7Mww8/3NPVkCSpW0TEX+td1y56SZIqyICXJKmCDHhJkipopT4GL0lq21tvvcXMmTN5/fW2Lqan3qpfv34MGjSIvn37rnAZBrwkVdTMmTPp378/gwcPprgwn1YGmcm8efOYOXMmW2yxxQqXYxe9JFXU66+/zvrrr2+4r2QigvXXX7/TPS8GvCRVmOG+cuqK582AlyQ1TFNTE0OHDmXbbbdlyJAhfP/73+edd94B4OGHH+a0005rdbvBgwczd+7cTu//xhtv5Iknnuh0OcvjIx/5CPPnz+/WfbbGY/CStKro6tZ8HRcrW3PNNZkyZQoAs2fP5hOf+AQLFizgvPPOY9iwYQwbNqxr69TCjTfeyKhRo9hmm226tNy3336bpqamVpfddtttXbqvFWULXpLULTbccEOuuOIKfvjDH5KZTJo0iVGjRgEwb9489t9/f3bccUdOOukk2rrS6dprr81Xv/pVhgwZwm677cYLL7wAwF//+lf23XdfdthhB/bdd1+ee+457rvvPiZOnMgXv/hFhg4dytNPP71UWTfccAPbbbcdQ4YMYcSIEQBcffXVnHrqqUvWGTVqFJMmTVqy769//evsuuuufOtb3+LII49cst6kSZM45JBDgHd7H7785S9z2WXvXtn63HPP5eKLLwbgoosuYvjw4eywww6cc845nXlY22TAS5K6zfvf/37eeecdZs+evdT88847jz333JNHH32U0aNH89xzz7W6/aJFi9htt92YOnUqI0aM4MorrwTg1FNP5dhjj+Wxxx7j6KOP5rTTTuPDH/4wo0eP5qKLLmLKlClsueWWS5V1/vnnc8cddzB16lQmTpzYYd0XLVrEdtttx4MPPshZZ53FAw88wKJFiwC47rrrGDNmzFLrjx07luuuu27J9PXXX88RRxzBnXfeyfTp03nooYeYMmUKkydP5p577un4wVtOBrwkqVu11jq/5557+OQnPwnAwQcfzLrrrtvqtquvvvqSVv/OO+/MX/7yFwDuv/9+PvGJTwBwzDHHcO+993ZYjz322IPjjz+eK6+8krfffrvD9Zuamvj4xz8OQJ8+fTjwwAO5+eabWbx4MbfeeiuHHnroUuvvuOOOzJ49m+eff56pU6ey7rrrstlmm3HnnXdy5513suOOO7LTTjvx5JNPMn369A73v7w8Bi9J6jbPPPMMTU1NbLjhhkybNm2pZfWMHO/bt++S9Zqamli8eHGr69VT1uWXX86DDz7IrbfeytChQ5kyZQp9+vRZMggQWOqnav369VvquPuYMWP40Y9+xHrrrcfw4cPp37//Mvs4/PDDmTBhAn//+98ZO3YsUHzBOeusszjppJM6rGNn2IKvFeFfd/1JWuXMmTOHk08+mVNPPXWZAB4xYgTXXnstALfffjsvvfTScpX94Q9/mPHjxwNw7bXXsueeewLQv39/Fi5c2Oo2Tz/9NLvuuivnn38+G2ywATNmzGDw4MFMmTKFd955hxkzZvDQQw+1uc+RI0fyyCOPcOWVVy7TPd9s7NixjB8/ngkTJnD44YcDcMABB/DTn/6UV155BYC//e1vyxyy6Aq24CVJDfPaa68xdOhQ3nrrLfr06cMxxxzDGWecscx655xzDkcddRQ77bQTe++9N5ttttly7efSSy/l05/+NBdddBEDBw7kP//zP4EiYD/zmc9w6aWXMmHChKWOw3/xi19k+vTpZCb77rsvQ4YMAWCLLbZg++23Z7vttmOnnXZqc59NTU2MGjWKq6++mnHjxrW6zrbbbsvChQvZZJNN2GijjQDYf//9mTZtGrvvvjtQDN675ppr2HDDDZfrPnck2hqpuDIYNmxYdun14G1Zdp+V+HUnrSymTZvG1ltv3dPV0Apq7fmLiMmZWddvC+2ilySpggx4SZIqyICXJKmCDHhJkirIgJckqYIMeEmSKsiAlyQ11De/+U223XZbdthhB4YOHcqDDz7Y6TInTpzIhRde2AW1K36HXkWe6EaSVhFxXtee6yPP6fh8Fvfffz+33HILjzzyCGussQZz587lzTffrKv8xYsX06dP6zE1evRoRo8evVz1XdXYgpckNcysWbPYYIMNWGONNQDYYIMN2HjjjZdcUhXg4YcfZuTIkUBxSdUTTzyR/fffn2OPPZZdd92Vxx9/fEl5I0eOZPLkyUsu67pgwQIGDx685Pzxr776KptuuilvvfUWTz/9NAceeCA777wze+21F08++SQAzz77LLvvvjvDhw/n7LPP7sZHo3sZ8JKkhtl///2ZMWMGH/zgBznllFO4++67O9xm8uTJ3HTTTfz85z9n7NixXH/99UDxZeH5559n5513XrLuOuusw5AhQ5aUe/PNN3PAAQfQt29fTjzxRH7wgx8wefJkvve973HKKacAcPrpp/PZz36WP/zhD/zDP/xDA+5172DAS5IaZu2112by5MlcccUVDBw4kDFjxnD11Ve3u83o0aNZc801ATjyyCO54YYbgHevp97SmDFjllx3ffz48YwZM4ZXXnmF++67jyOOOIKhQ4dy0kknMWvWLAB+//vfc9RRRwHFpWWrymPwkqSGampqYuTIkYwcOZLtt9+ecePGLXVZ1tpLsgKstdZaS25vsskmrL/++jz22GNcd911/OQnP1mm/NGjR3PWWWfx4osvMnnyZPbZZx8WLVrEgAEDmDJlSqt1qudysis7W/CSpIb585//zPTp05dMT5kyhc0335zBgwczefJkAH75y1+2W8bYsWP57ne/y4IFC9h+++2XWb722muzyy67cPrppzNq1Ciampp473vfyxZbbLGk9Z+ZTJ06FYA99thjqUvLVpUBL0lqmFdeeYXjjjuObbbZhh122IEnnniCc889l3POOYfTTz+dvfbai6ampnbLOPzwwxk/fjxH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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x7f1088134d30>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "vs.survival_stats(data, outcomes, 'Sex')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Examining the survival statistics, a large majority of males did not survive the ship sinking. However, a majority of females *did* survive the ship sinking. Let's build on our previous prediction: If a passenger was female, then we will predict that they survived. Otherwise, we will predict the passenger did not survive.  \n",
    "Fill in the missing code below so that the function will make this prediction.  \n",
    "**Hint:** You can access the values of each feature for a passenger like a dictionary. For example, `passenger['Sex']` is the sex of the passenger."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "def predictions_1(data):\n",
    "    \"\"\" Model with one feature: \n",
    "            - Predict a passenger survived if they are female. \"\"\"\n",
    "    \n",
    "    predictions = []\n",
    "    for _, passenger in data.iterrows():\n",
    "        \n",
    "        # Remove the 'pass' statement below \n",
    "        # and write your prediction conditions here\n",
    "        sex = passenger['Sex']\n",
    "        if sex == 'male':\n",
    "            predictions.append(0)\n",
    "        else:\n",
    "            predictions.append(1)\n",
    "\n",
    "    # Return our predictions\n",
    "    return pd.Series(predictions)\n",
    "\n",
    "# Make the predictions\n",
    "predictions = predictions_1(data)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Question 2\n",
    "\n",
    "* How accurate would a prediction be that all female passengers survived and the remaining passengers did not survive?\n",
    "\n",
    "**Hint:** Run the code cell below to see the accuracy of this prediction."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Predictions have an accuracy of 78.68%.\n"
     ]
    }
   ],
   "source": [
    "print(accuracy_score(outcomes, predictions))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Answer**: *Replace this text with the prediction accuracy you found above.*"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "***\n",
    "Using just the **Sex** feature for each passenger, we are able to increase the accuracy of our predictions by a significant margin. Now, let's consider using an additional feature to see if we can further improve our predictions. For example, consider all of the male passengers aboard the RMS Titanic: Can we find a subset of those passengers that had a higher rate of survival? Let's start by looking at the **Age** of each male, by again using the `survival_stats` function. This time, we'll use a fourth parameter to filter out the data so that only passengers with the **Sex** 'male' will be included.  \n",
    "Run the code cell below to plot the survival outcomes of male passengers based on their age."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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5/L9738fgPPPP5/dd9+dnXfemXPOOaczb2uHTPaSpC619dZb8/bbbzN37txlxp933nnsu+++3HfffYwaNYqnnnqqzeUXLVrEXnvtxYwZM9h///255JJLADjttNM48cQTuf/++zn++OM5/fTTef/738+oUaM4//zzmT59Ou95z3uWWdfXv/51br75ZmbMmMHkyZM7jH3RokXsuOOOTJkyhTPPPJN77rmHRYsWAXDFFVcwevToZeYfM2YMV1xxxZLhK6+8kmOOOYZbbrmFWbNmce+99zJ9+nSmTZvGnXfe2fGbt4pM9pKkLtdWqf3OO+/kYx/7GACHH344G2ywQZvLrrnmmktqA3bbbTeefPJJAO6++24++tGPAnDCCSfwhz/8ocM49tlnHz7+8Y9zySWX8NZbb3U4f58+ffjIRz4CQN++fTn00EO57rrrWLx4MTfccANHHnnkMvPvsssuzJ07l2eeeYYZM2awwQYbsOWWW3LLLbdwyy23sMsuu7Drrrvy8MMPM2vWrA63v6q8Zi9J6lKPP/44ffr0YZNNNmHmzJnLTKunBXq/fv2WzNenTx8WL17c5nz1rOuiiy5iypQp3HDDDQwbNozp06fTt2/fJQ0IgWVuf+vfv/8y1+lHjx7NT37yEzbccEN233131l9//eW2cfTRR3P11Vfz3HPPMWbMGKD4sXPmmWcyttFtKFbAZK/u00UHebepo+GStLqZN28en/rUpzjttNOWS8b7778/EydO5KyzzuKmm27ipZdeWql1v//972fSpEmccMIJTJw4kX333ReA9ddfn1deeaXNZR577DH23HNP9txzT6677jpmz57N4MGDufDCC3n77bd5+umnuffee1e4zREjRnDKKadwySWXLFeF32LMmDF88pOf5IUXXuCOO+4A4JBDDuHss8/m+OOPZ7311uPpp5+mX79+bLLJJiu1z/Uy2UuSmur1119n2LBhvPnmm/Tt25cTTjiBz3/+88vNd84553Dcccex6667csABB7Dllluu1HYuuOACPvGJT3D++eczcOBAfvGLXwBLk+0FF1zA1Vdfvcx1+y9+8YvMmjWLzOSggw5i6NChAAwZMoSddtqJHXfckV133XWF2+zTpw8jR47ksssuY8KECW3Os8MOO/DKK6+w+eabs+mmmwJw8MEHM3PmTPbee2+gaPj3y1/+smnJPlbU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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x7fa0607d9dd8>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "vs.survival_stats(data, outcomes, 'Age', [\"Sex == 'male'\"])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "Examining the survival statistics, the majority of males younger than 10 survived the ship sinking, whereas most males age 10 or older *did not survive* the ship sinking. Let's continue to build on our previous prediction: If a passenger was female, then we will predict they survive. If a passenger was male and younger than 10, then we will also predict they survive. Otherwise, we will predict they do not survive.  \n",
    "Fill in the missing code below so that the function will make this prediction.  \n",
    "**Hint:** You can start your implementation of this function using the prediction code you wrote earlier from `predictions_1`."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "ename": "NameError",
     "evalue": "name 'data' is not defined",
     "output_type": "error",
     "traceback": [
      "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[0;31mNameError\u001b[0m                                 Traceback (most recent call last)",
      "\u001b[0;32m<ipython-input-2-0b35b38c2a5c>\u001b[0m in \u001b[0;36m<module>\u001b[0;34m()\u001b[0m\n\u001b[1;32m     23\u001b[0m \u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     24\u001b[0m \u001b[0;31m# Make the predictions\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 25\u001b[0;31m \u001b[0mpredictions\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mpredictions_2\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mdata\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m",
      "\u001b[0;31mNameError\u001b[0m: name 'data' is not defined"
     ]
    }
   ],
   "source": [
    "def predictions_2(data):\n",
    "    \"\"\" Model with two features: \n",
    "            - Predict a passenger survived if they are female.\n",
    "            - Predict a passenger survived if they are male and younger than 10. \"\"\"\n",
    "    \n",
    "    predictions = []\n",
    "    for _, passenger in data.iterrows():\n",
    "        \n",
    "        # Remove the 'pass' statement below \n",
    "        # and write your prediction conditions here\n",
    "        l_sex = passenger['Sex']\n",
    "        l_age = passenger['Age']\n",
    "        if l_sex == 'male':\n",
    "            if l_age <= 10 :\n",
    "                predictions.append(1)\n",
    "            else:\n",
    "                predictions.append(0)\n",
    "        else:\n",
    "            predictions.append(1)\n",
    "    \n",
    "    # Return our predictions\n",
    "    return pd.Series(predictions)\n",
    "\n",
    "# Make the predictions\n",
    "predictions = predictions_2(data)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Question 3\n",
    "\n",
    "* How accurate would a prediction be that all female passengers and all male passengers younger than 10 survived? \n",
    "\n",
    "**Hint:** Run the code cell below to see the accuracy of this prediction."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Predictions have an accuracy of 79.24%.\n"
     ]
    }
   ],
   "source": [
    "print(accuracy_score(outcomes, predictions))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Answer**: *Replace this text with the prediction accuracy you found above.*"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "***\n",
    "Adding the feature **Age** as a condition in conjunction with **Sex** improves the accuracy by a small margin more than with simply using the feature **Sex** alone. Now it's your turn: Find a series of features and conditions to split the data on to obtain an outcome prediction accuracy of at least 80%. This may require multiple features and multiple levels of conditional statements to succeed. You can use the same feature multiple times with different conditions.   \n",
    "**Pclass**, **Sex**, **Age**, **SibSp**, and **Parch** are some suggested features to try.\n",
    "\n",
    "Use the `survival_stats` function below to to examine various survival statistics.  \n",
    "**Hint:** To use mulitple filter conditions, put each condition in the list passed as the last argument. Example: `[\"Sex == 'male'\", \"Age < 18\"]`"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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0047bdUwEyZMYMaMGavm/ZWvfIU999yTr3/96xx99NGrhpsxYwaHH3448PZWhC9+8Yv84Adv3035rLPO4tvf/jYA5557LmPHjmXXXXdl6tSp3VmtbTLEJUk9bvvtt+ett95i4cKFq71+9tlns99++3HfffcxceJEnnrqqTbHX758OXvttRf3338/48aN4+KLLwbgtNNO4xOf+AQPPPAAxx57LKeffjr77LMPEydO5Nxzz2XOnDm8+93vXm1a55xzDr/61a+4//77ueGGGzqtffny5ey8887cddddnHnmmdx5550sX74cgKuuuopJkyatNvzkyZO56qqrVnVfffXVHHXUUdx6663MmzePu+++mzlz5jB79mxmzpzZ+crrAkNcklQXbbWyZ86cyXHHHQfAYYcdxqabbtrmuOuuu+6q1vsee+zBk08+CcAdd9zBxz/+cQCOP/54brvttk7r2HfffTnhhBO4+OKLWblyZafDNzU18bGPfQyAAQMGcOihh3LjjTeyYsUKbrrpJo444ojVht9tt91YuHAhzzzzDPfffz+bbropw4cP59Zbb+XWW29lt912Y/fdd+eRRx5h3rx5nc6/K9wnLknqcY8//jhNTU1sueWWzJ07d7V+tRyRPXDgwFXDNTU1sWLFijaHq2VaF154IXfddRc33XQTo0ePZs6cOQwYMGDVgXfAaqd5DRo0aLX94JMmTeKCCy5gs802Y+zYsQwZMuQd8zjyyCOZPn06zz33HJMnTwaKHzFnnnkmJ598cqc1rqn+2RKPqO9DkvqxRYsWccopp3Daaae9I2THjRvHFVdcAcAtt9zCSy+91KVp77PPPkybNg2AK664gv322w+AIUOGsHTp0jbHeeyxx9hzzz0555xz2GKLLZg/fz4jRoxgzpw5vPXWW8yfP5+777673XmOHz+ee++9l4svvvgdm9KbTZ48mWnTpjF9+nSOPPJIAA455BB+/OMfs2zZMgCefvrpd+xe6C5b4pKkbnvttdcYPXo0b775JgMGDOD444/nc5/73DuGmzp1Kscccwy77747+++/P8OHD+/SfL773e/yqU99inPPPZehQ4fyk5/8BChC9NOf/jTf/e53mT59+mr7xT//+c8zb948MpMDDzyQUaNGATBy5Eh22WUXdt55Z3bfffd259nU1MSECRO49NJLueyyy9ocZqeddmLp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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x7f1060d86fd0>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "vs.survival_stats(data, outcomes, 'Parch', [\"Sex == 'male'\", \"Age < 18\", \"SibSp == 0\"])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "After exploring the survival statistics visualization, fill in the missing code below so that the function will make your prediction.  \n",
    "Make sure to keep track of the various features and conditions you tried before arriving at your final prediction model.  \n",
    "**Hint:** You can start your implementation of this function using the prediction code you wrote earlier from `predictions_2`."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [],
   "source": [
    "def predictions_3(data):\n",
    "    \"\"\" Model with multiple features. Makes a prediction with an accuracy of at least 80%. \"\"\"\n",
    "    \n",
    "    predictions = []\n",
    "    for _, passenger in data.iterrows():\n",
    "        \n",
    "        # Remove the 'pass' statement below \n",
    "        # and write your prediction conditions here\n",
    "        l_sex = passenger['Sex']\n",
    "        l_age = passenger['Age']\n",
    "        l_class = passenger['Pclass']\n",
    "        l_child = passenger['Parch']\n",
    "        l_sib = passenger['SibSp']\n",
    "        l_embarked = passenger['Embarked']\n",
    "        if l_sex == 'male':\n",
    "            if l_sib == 0 and l_child > 0 and l_age < 18:\n",
    "                predictions.append(1)\n",
    "            elif(l_child > 2 or l_sib > 2):\n",
    "                predictions.append(0)\n",
    "            elif (l_age <= 10) or (l_class == 1 and l_age < 18):\n",
    "                predictions.append(1)\n",
    "            else:\n",
    "                predictions.append(0)\n",
    "        else:\n",
    "            if l_sib > 4:\n",
    "                predictions.append(0) \n",
    "            else:\n",
    "                predictions.append(1)\n",
    "    \n",
    "    # Return our predictions\n",
    "    return pd.Series(predictions)\n",
    "\n",
    "# Make the predictions\n",
    "predictions = predictions_3(data)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Question 4\n",
    "\n",
    "* Describe the steps you took to implement the final prediction model so that it got **an accuracy of at least 80%**. What features did you look at? Were certain features more informative than others? Which conditions did you use to split the survival outcomes in the data? How accurate are your predictions?\n",
    "\n",
    "**Hint:** Run the code cell below to see the accuracy of your predictions."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Predictions have an accuracy of 81.37%.\n"
     ]
    }
   ],
   "source": [
    "print(accuracy_score(outcomes, predictions))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Answer**: *Replace this text with your answer to the question above.*"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Conclusion\n",
    "\n",
    "After several iterations of exploring and conditioning on the data, you have built a useful algorithm for predicting the survival of each passenger aboard the RMS Titanic. The technique applied in this project is a manual implementation of a simple machine learning model, the *decision tree*. A decision tree splits a set of data into smaller and smaller groups (called *nodes*), by one feature at a time. Each time a subset of the data is split, our predictions become more accurate if each of the resulting subgroups are more homogeneous (contain similar labels) than before. The advantage of having a computer do things for us is that it will be more exhaustive and more precise than our manual exploration above. [This link](http://www.r2d3.us/visual-intro-to-machine-learning-part-1/) provides another introduction into machine learning using a decision tree.\n",
    "\n",
    "A decision tree is just one of many models that come from *supervised learning*. In supervised learning, we attempt to use features of the data to predict or model things with objective outcome labels. That is to say, each of our data points has a known outcome value, such as a categorical, discrete label like `'Survived'`, or a numerical, continuous value like predicting the price of a house."
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 1
}
                                                                                                                                                                                                                          ./visuals.py                                                                                        0000644 0000000 0000000 00000013317 13225527476 011773  0                                                                                                    ustar   root                            root                                                                                                                                                                                                                   ###########################################
# Suppress matplotlib user warnings
# Necessary for newer version of matplotlib
import warnings
warnings.filterwarnings("ignore", category = UserWarning, module = "matplotlib")
#
# Display inline matplotlib plots with IPython
from IPython import get_ipython
get_ipython().run_line_magic('matplotlib', 'inline')
###########################################

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

def filter_data(data, condition):
    """
    Remove elements that do not match the condition provided.
    Takes a data list as input and returns a filtered list.
    Conditions should be a list of strings of the following format:
      '<field> <op> <value>'
    where the following operations are valid: >, <, >=, <=, ==, !=
    
    Example: ["Sex == 'male'", 'Age < 18']
    """

    field, op, value = condition.split(" ")
    
    # convert value into number or strip excess quotes if string
    try:
        value = float(value)
    except:
        value = value.strip("\'\"")
    
    # get booleans for filtering
    if op == ">":
        matches = data[field] > value
    elif op == "<":
        matches = data[field] < value
    elif op == ">=":
        matches = data[field] >= value
    elif op == "<=":
        matches = data[field] <= value
    elif op == "==":
        matches = data[field] == value
    elif op == "!=":
        matches = data[field] != value
    else: # catch invalid operation codes
        raise Exception("Invalid comparison operator. Only >, <, >=, <=, ==, != allowed.")
    
    # filter data and outcomes
    data = data[matches].reset_index(drop = True)
    return data

def survival_stats(data, outcomes, key, filters = []):
    """
    Print out selected statistics regarding survival, given a feature of
    interest and any number of filters (including no filters)
    """
    
    # Check that the key exists
    if key not in data.columns.values :
        print("'{}' is not a feature of the Titanic data. Did you spell something wrong?".format(key))
        return False

    # Return the function before visualizing if 'Cabin' or 'Ticket'
    # is selected: too many unique categories to display
    if(key == 'Cabin' or key == 'PassengerId' or key == 'Ticket'):
        print("'{}' has too many unique categories to display! Try a different feature.".format(key))
        return False

    # Merge data and outcomes into single dataframe
    all_data = pd.concat([data, outcomes.to_frame()], axis = 1)
    
    # Apply filters to data
    for condition in filters:
        all_data = filter_data(all_data, condition)

    # Create outcomes DataFrame
    all_data = all_data[[key, 'Survived']]
    
    # Create plotting figure
    plt.figure(figsize=(8,6))

    # 'Numerical' features
    if(key == 'Age' or key == 'Fare'):
        
        # Remove NaN values from Age data
        all_data = all_data[~np.isnan(all_data[key])]
        
        # Divide the range of data into bins and count survival rates
        min_value = all_data[key].min()
        max_value = all_data[key].max()
        value_range = max_value - min_value

        # 'Fares' has larger range of values than 'Age' so create more bins
        if(key == 'Fare'):
            bins = np.arange(0, all_data['Fare'].max() + 20, 20)
        if(key == 'Age'):
            bins = np.arange(0, all_data['Age'].max() + 10, 10)
        
        # Overlay each bin's survival rates
        nonsurv_vals = all_data[all_data['Survived'] == 0][key].reset_index(drop = True)
        surv_vals = all_data[all_data['Survived'] == 1][key].reset_index(drop = True)
        plt.hist(nonsurv_vals, bins = bins, alpha = 0.6,
                 color = 'red', label = 'Did not survive')
        plt.hist(surv_vals, bins = bins, alpha = 0.6,
                 color = 'green', label = 'Survived')
    
        # Add legend to plot
        plt.xlim(0, bins.max())
        plt.legend(framealpha = 0.8)
    
    # 'Categorical' features
    else:
       
        # Set the various categories
        if(key == 'Pclass'):
            values = np.arange(1,4)
        if(key == 'Parch' or key == 'SibSp'):
            values = np.arange(0,np.max(data[key]) + 1)
        if(key == 'Embarked'):
            values = ['C', 'Q', 'S']
        if(key == 'Sex'):
            values = ['male', 'female']

        # Create DataFrame containing categories and count of each
        frame = pd.DataFrame(index = np.arange(len(values)), columns=(key,'Survived','NSurvived'))
        for i, value in enumerate(values):
            frame.loc[i] = [value, \
                   len(all_data[(all_data['Survived'] == 1) & (all_data[key] == value)]), \
                   len(all_data[(all_data['Survived'] == 0) & (all_data[key] == value)])]

        # Set the width of each bar
        bar_width = 0.4

        # Display each category's survival rates
        for i in np.arange(len(frame)):
            nonsurv_bar = plt.bar(i-bar_width, frame.loc[i]['NSurvived'], width = bar_width, color = 'r')
            surv_bar = plt.bar(i, frame.loc[i]['Survived'], width = bar_width, color = 'g')

            plt.xticks(np.arange(len(frame)), values)
            plt.legend((nonsurv_bar[0], surv_bar[0]),('Did not survive', 'Survived'), framealpha = 0.8)

    # Common attributes for plot formatting
    plt.xlabel(key)
    plt.ylabel('Number of Passengers')
    plt.title('Passenger Survival Statistics With \'%s\' Feature'%(key))
    plt.show()

    # Report number of passengers with missing values
    if sum(pd.isnull(all_data[key])):
        nan_outcomes = all_data[pd.isnull(all_data[key])]['Survived']
        print("Passengers with missing '{}' values: {} ({} survived, {} did not survive)".format( \
              key, len(nan_outcomes), sum(nan_outcomes == 1), sum(nan_outcomes == 0)))

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 