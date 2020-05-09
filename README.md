# COMP2113GroupProject
This is the repo for COMP2113 Group Project (group 157)
## Group Member
Tan Lihui, Yuan Jing
## Requirements (temporal)
1. Generation of random game sets or events
2. Data structures for storing game status
3. Dynamic memory management
4. File input/output (e.g., for loading/saving game status)
5. Program codes in multiple files
6. Proper indentation and naming styles
7. In-code documentation

-Commit comment should not be empty and should be written sensibly.

-For each function, comments on “what it does”, “what the inputs are” and “what the outputs are” are needed.

-You may use any of the C/C++ libraries.
## Game Description
This is a simple round-based strategy, player-versus-player game. Setting in a war background, player acts as the commander of a country, who controls the army and factories. 
Once the game starts, it will display a 2D grid map. Each grid has a terrain feature which is not changable. Different types of terrain have different features, one of which could be occupied by troops or buildings(which are called Occupiers).  
Two players could act in turn, including moving troops(maybe starting a battle), healing soldiers/building, gernerating troops, updating factories.  
Note: The player can just choose make one change in one turn. i.e. He cannot do two things in one turn.  
Players should try his best to manage his army and factories to occupy enermy's capital.

## Rules: 
### New game and load previous record:  
Before starting playing, players can choose to start a new game or reload previous record.  
However, the players need to enter the map/record file name (name without extension).  
When players quit in the middle of the game, status of one game can be chosed to store as .txt files in ./savings/.

### Control:
-use "w", "s", "a", "d" to move your cursor up, down, left, right respectively.  
-press "e" to confirm / proceed. It is used to choose occupiers, destinations, etc.
-press "q" to return to previous step.
-press esc to quit game.  

### Terrain:
There will be three types of terrains.   
-Land(white `地` or white square): normal area that can be occupied with troops or factories. Each land has its peoductivity level.  
-Sea(blue `海` or blue square): forbidden area, which means player cannot choose it as troops' destination. Nothing can access this area.  
-Hill(green `山` or green square): forbidden area. Same as Sea. 


### Buildings:
-Factory(`厰`) is build with an engineer with 1000 money. It has the function of generating troops. Factories have an attack point of 1 and life point of 20.  
-Capital(`都`) is the heart of one country(one player). It also has the function of generating troops. Capitals have an attack point of 1 and life point of 20. More importantly, Once the capital is occupied by the enermy（the other player), you lose.  

-Generating new troops: Players can choose to generate new troops in a building's neighbouring area: 3* 3 grids whose center is the building. 
-Attack point: the hurt level it can cause to its attacher in each winning round.  
-Life point: How many hurt it can bear before destroyed. 

### Money:
-Money is accumulatively generated each round by the sum of the productivity of terrain where this player's capital and factories occupy. 
-Two players each will have 200 money initially. (This can be altered manually by changing the map text files) 

### Troops:
There are four types of troops with different functions or capabilities.  
-Soldier(`兵`) soldiers have an attack point of 2, a life of 2, and a portability of 1, and a cost of 100.  
-Engineer(`工`): engineers have an attack point of 1, a life of 2, and a portability of 1, and a cost of 150. Its primary function is to build a factory, which means players can spend money to update `工` to `厰`.  
-Light Armoured Vehicle(LAV) (`車`): LAVs have an attack point of 4, a life of 3, and a portability of 2, and a cost of 300.  
-Tank (`炮`): Tanks have an attack point of 6, a life of 5, and a portability of 1, and a cost of 600. 
 
-Attack point: the hurt level it can cause to his enermy in each winning round.  
-Life point: How many hurt points it can bear before its death.  
-Portability:  
1: it can move to 3* 3 grids whose center is the troops' location.  
2: it can move to 5* 5 grids whose center is the troops' location.

### Battle procedure:
In a certain battle, it could have several rounds to cause one's death, which is an end of a battle.  
Each round's result is a random event generated by computer.  
In each round, the winer will cause the loser's life point decrease by the attack point of the winner.  
Once one side's life point is 0 (or < 0 in coding level), it dies. The other side wins and occupies this grid.

### Recover Life:
Both Buildings and troops' life level can be recovered to its maximum level with the same cost of generating it. Additionlly, The recover life cost of capital is 1000.

### Winning condition:
One player occupies the other one's capital.

## Features
1. Immediate control: the whole process (including start page, game page, exit page, etc. ) is controlled by keyboard events. Various listeners are used to keep track of user control in different stages. Upon keyboard events, screen manager will conduct behaviors such as refreshing screen or updating certain information on the screen. 

2. Graphical game interface: the game utilises symbols and ANSI escape sequence to implement a colorful graphical map and different Occupiers (buildings and troops).

3. Process (partially) organised in classes: this segments the whole game process into work achieved by different objects. Like functions, it also simplifies co-ordination between groupmates. Classes are more convient in that it can include many data and functions. One may simply include a header file without implementation in order to use features not yet implemented by the other groupmate.

#### 4. Random game settings and events:
In each battle, the two sides (attacker & defender) will perform multiple attacks, in each attack, chance decides who attacks whom. This is assure a better balance between different types of troops. 

#### 5. Data structures for storing game status: 
##### Most data are organized in classes, for example:
-location(x, y), graphical representation, type of Point(soldier, engineer, tank, ...), attack, life, etc. in Point. 
-part of the data above stored in Passer classes.
-New Occupier data stored in Initialiser and ConfigInitiator.
-Game status (turn-counter, money-counter) stored in GameMediator
##### Two-dimensional Array of Point* used in Map to store the game map (like a chessboard)
##### Struct used to organize data as a "package"
##### Union used in Point so as to save memory

#### 6. Dynamic memory management: 
##### The two-dimensional array map is `new`ed according to the height and width stated in maps or savings files.
##### After a Point is discarded (Occupier destroyed or Terrain occupierd), the union Pass and the information OccupierPasser / TerrainPasser in the union will be `delete`ed so as to release the memory. 
##### Other data, such as the array in Map, initialization data in Initialiser and ConfigInitiator and game status in GameMediator are also deleted (in destructors); however, these all live until the end of the program, therefore it is not that important.

#### 7. File input/output
-input: maps or game status records (from archive at last exit time).  
-output: to archive game status when exiting with save chosen.  

#### 8. Program codes in multiple files: 
Different functions and classes packed in different .cpp files, they rely on each other via .h files.


9. 
Presumably, we use objects to represent the troops, factories and terrains. They all derive from one base class.  
Use a two-dimensional array of the base class to respent map (each element, or point, represents one area).  

The base class (`Point`) has `int x` and `int y`, representing the coordinates of this point, and a `string representation`, defining the representation of this point on the map. It has a `toLocation()` method, which updates the responsive location in the map.  

`Terrain` derives from `Point`, it has a string attribute (`string type`), defining the type of terrain; it also has a `int productivity` attribute that provides information about how much money this area can generate each round. It also has a `toStorage()` method, which returns a storage object to an occupier (a troop or a factory) for storing the original type of terrain and its productivity.  

`Occupier` derives from `Point` as well, it has a `Storage was` attribute, storing the terrain information of the area it occupies. It also has a `range` attribute, denoting the range it covers (ex. 1 for soldiers, engineers, tanks and factories, 2 for LAVs). In addition, it has `attackPoint` (refers to max amount of troops produceable by factories) and `life` attributes. It has `enter()` and `leave()` method that deals with the occupier moving to and leaving one point. It has a virtual `die()` method that requires further implementation accordingly.  

`Troop` derives from `Occupier`. It has an additional attribute `int number`, denoting the number of troops on one point (note that only one type of troops is allowed on one point). It has `moveTo(int x, int y)` method, which changes the `x` and `y`, restores the original terrain from `was`, calls `leave()` and goes to the new point by calling `toLocation()`. There is also an `initiateBattle(int x, int y)`, which is called automatically upon the order of moving to a point with an `Occupier`. This method calls a `BattleManager` to draw a random number representing luck and calculate the total attack power of this troop and passes it to the `receiveBattle(type enemyAttackPower)` method of the enemy `Occupier`. Upon receiving the attack, the `receiveBattle()` does the same as the former and passes the `enemyAttackPower` to the `die()` method of the initiater, and then calls `die()` of itself. (**this requires further designing**). Note that `die()` of Troops counts down the `number` first and when `number = 0`, calls `leave()`.  

`Factory` derives from `Occupier` as well. It has a series `makeX()` methods for producing new troops in the neiboring regions. It should also implement a `die()` method.  
