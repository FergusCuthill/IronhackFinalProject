# IronhackFinalProject

Project to create a game recommendation engine for Ironhack final project submission. 

Engine uses a colaborative filtering approach to recommend games to a user based on a dataset of 300,000 sampled users. 


## Source Data

Details of games were extracted from the Steam Store API using a modified version of the code from Nik Davis here:
https://nik-davis.github.io/posts/2019/steam-data-collection/

Steam user data was from a dataset collected in 2014 by Brigham Young university which can be accessed here:
https://steam.internet.byu.edu/
Unfortunately, this dataset is from 2014 which limits the recommendation engine to recommending games from 2014 and earlier. I could find no more up to date datasets than this. 


## Methodology

All data cleaning is done in python and the code can be found in the data cleaning portion of this repository. 
As the dataset of Steam User data is very large, data for 300,000 users is sampled from it instead and then only games that are in the current list of games extracted from the API were included. This data is then stored in SciPy Sparse Matricies for more efficient access. 
The model uses a colaborative filtering approach where it compares the games owned by the user to games owned by each of the sampled other users and computes a similarity score for each of the sampled users based on the following formula:

Score = Matches**2 / (UGO * SGO +1)

Where:
Matches = Number of games owned by both parties
UGO = Number of games owned by the user
SGO = Number of games owned by the sampled user

This is effectively the proportion of the users games owned by the sampled user multiplied by the proportion of the sampled users games owned by the user. 
The top 30 most similar users are then extracted and the similarity score applied to the games they own. A sum is then computed for each game and the games already owned by the user are filtered out (though checking that the recommendation engine is recommending games owned by the user before filtering is a useful test). The highest scoring games are then listed with details about the games. 
The user is also able to further filter the list for languages and age restrictions. 


## Outcome

Despite the engine being unable to recommend games more recent than 2014, I still ended up buying (and enjoying) one of the games it recommended and there are several more I've been meaning to try (though I might try more recent entries in those series). 

A few upgrades would be needed to make a more effective recommendation engine:
Newer dataset - limit of 2014 is a problem
Exclusion of DLC for games not owned - List can get a bit clogged with DLC and it makes little sense to recommend DLC to a user who does not own the base game
User Interface - Currently, this is solely accessed through Python. In addition, a user must manually create an excel file of the IDs of games owned. I could find no better way of downloading the App IDs for games owned without changing privacy settings and downloading via the API. 
