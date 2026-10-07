Design 

D1 Decomposition Diagram 

Figure 1: Decomposition Diagram of FIFA Simulator 

 

D2 Structure Design 

2.1 Overall System Structure 

The FIFA World Cup Simulator will be structured as an object-oriented Python program consisting of four main classes: Team, Match, Group, and Tournament. The Tournament class will manage the overall tournament, including the groups and knockout stages. Each Group will contain four Team objects and the matches played between them. The Match class will manage an individual match between two teams, including its simulated result. The Team class will store information about an individual team, including its rating and tournament statistics. The system will also use CSV files for permanent data storage mentioned in 2.3 Permanent data storage .Team data will be loaded from a CSV file when the program starts and validated before being used to create Team objects. Match and tournament results can then be processed during execution and exported to a CSV file for permanent storage. This structure separates the different responsibilities of the system, making the program easier to develop, test, and maintain. 

2.2 IPSO Chart 

The IPSO diagram shows how data moves through my simulator from user and CSV inputs, through the processing performed by the tournament algorithms, to the stored data and outputs presented to the user. It shows the main inputs, processes, storage requirements, and outputs of the completed system. 

 

Figure 2: IPSO Chart of FIFA Simulator 

2.3 Permanent data storage 

The simulator will use CSV (Comma Separated Values) files for permanent data storage. A teams.csv file will store the initial team data, including each team’s name, identifier and rating. The program will load and validate this data when the simulator starts. Tournament results will be exported to a separate CSV file so that completed simulations can be stored and reviewed after the program has ended. CSV has been selected because the data is tabular and relatively small, meaning a full relational database would add unnecessary complexity without providing any significant benefit to the requirements of my system mentioned in A6 Requirements. 

 

2.4 User Interface design 

The simulator will use a terminal-based interface rather than a graphical user interface. This is appropriate because the primary purpose of the system is to simulate and analyse tournament outcomes rather than provide a visually complex user interface – this is not a game. A terminal interface allows the user to entire only requireed inputs, sleect simulation options and view tournament results whilst keeping the system lightweight and focused on its computational fucntionality. It also allows the same Python program to be develpoed and executed on different OS systems ike Windows computers at school and Linux/macOS environments at home during development and testing. Users can also access the program from different OS systems or virtual machines, removing any accessibility barriers. A graphical interface could be added in the future development, but it is not necessary to fulfil the requirements of my system, as my simulator only requires file uploads, storage, handling, user input, and return outputs. 

D3 Algorithm Design 

Algorithm 1: Match Simulation 

This algorithm is used to simulate realistic matches whilst ensuring the result is not deterministic. The difference between the two teams’ ratings is converted into a probability (decimal), meaning a higher-rated team has a bigger chance of winning while still allowing a lower-rated team to win. A random value is then generated and compared with the calculated probability to determine the likely result of the match. The match result and score are then recorded, allowing the simulator to represent a win, draw or loss rather than assuming that every match must produce a winner. This represents the uncertainty inherent in football and other factors that are not explicitly modelled by the simulator, such as recent form, match conditions or individual events, but their combined unpredictability is abstracted into the stochastic component of my model. This is appropriate because the objective of the simulator is to demonstrate how team strength and randomness affect tournament outcomes whilst still keeping the model simple enough to understand and use. The same Match Simulation algorithm can be reused for both group-stage and knockout matches, ensuring that the probability model is applied consistently throughout the tournament. 

Rating-based win Probability 

 

 

Where RA is Team A’s rating and RB is Team B’s rating. The rating difference determines the direction and size of the probability. If both teams have the same rating, the probability of Team A winning is 0.5. If Team A has a higher rating, its probability increases above 0.5, whilst if Team A has a lower rating, its probability decreases below 0.5. The probability of Team B is then 1-P(A). This allows team strength to influence the outcome without making the result deterministic. 

The probability calculation is adapted from the expected-score formula used in the Elo rating system. The Elo system uses the difference between two players' ratings to calculate an expected score using a logistic probability model. I have adapted this approach for the simulator by applying the same mathematical relationship to the ratings of two football teams. The simulator does not implement the Elo rating system itself, as team ratings are provided as input data and are not dynamically updated after each match. Instead, the formula is used only to convert the existing team ratings into probabilities for the simulated match outcome. 

The calculated probability is used to determine the relative likelihood of each team winning, but the match is not treated as deterministic. A random value is generated during the simulation so that the same two teams can produce different results across different simulations. The match can result in a win, draw or loss, and the resulting score is stored as part of the Match object. In a group-stage match, this score is then used by the Group Standings algorithm to calculate points, goals scored, goals conceded and goal difference. In a knockout match, a draw cannot allow both teams to progress, so an additional winner-determination process is required to select the team that progresses. 

PSEUDOCODE 

 

 

Algorithm 2: Group Stage Simulation 

This algorithm is used to simulate the group stage of the FIFA World Cup Simulator. The 32 teams are divided into 8 groups of 4 teams, with each team playing every other team in its group once. For each fixture, the Match Simulation algorithm is called to determine the result using the rating-based probability model. The result of each match and score is then recorded and the relevant team statistics are updated. Once all matches within a group have been simulated, the teams are ranked according to their accumulated results and the required teams are selected to progress to the knockout stage. This process is repeated for each of the 8 groups, resulting in 16 teams progressing from the group stage. This is appropriate because it models the structure of the World Cup group stage whilst reusing the Match Simulation algorithm rather than duplicating its probability calculations. Separating the group-stage process from the individual match simulation also makes the system easier to test and maintain. 

PSEUDOCODE 

 

 

With 4 teams in each group, the algorithm produces 6 unique fixtures per group, resulting in 48 group-stage matches across the 8 groups. 

This algorithm is appropriate because the nested loops systematically generate every unique fixture within each group without requiring individual matches to be manually defined. Calling the Match Simulation algorithm for each fixture ensures that the same probability model is consistently applied throughout the group stage. Recording the results and updating the team statistics after each match allows the completed group to be ranked and the qualifying teams to be identified. The algorithm therefore connects the individual Match Simulation process with the wider tournament structure. 

Algorithm 3: Group Standings 

This algorithm is used to calculate and rank the standings of the teams within each group after all group-stage matches have been simulated. The result and score of each match are used to update the relevant team's statistics. A team receives 3 points for a win, 1 point for a draw and 0 points for a loss. The goals scored and goals conceded in each match are also recorded, allowing the goal difference of each team to be calculated. Once all matches in the group have been processed, the teams are ranked primarily by points and then by the defined goal-based tie-breaking criteria where required. The highest-ranked two teams are selected to progress to the knockout stage. This is appropriate because the group stage requires the results of multiple matches to be combined into a single ranking, rather than simply identifying individual match winners. Separating the calculation of the standings from the Group Stage Simulation algorithm allows the match results to be processed consistently after all fixtures have been completed and makes the ranking process easier to test independently. 

PSEUDOCODE 

 

 

This algorithm is appropriate because it converts the individual results produced by the Match Simulation algorithm into the statistics required to rank a World Cup group. Processing every match ensures that all results contribute to the final standings, while calculating goal difference and goals scored provides additional criteria when teams have the same number of points. The algorithm is separated from Match Simulation so that the probability and result generation logic does not become mixed with the ranking logic. This makes each algorithm responsible for a specific part of the system and allows the standings calculation to be tested independently. Selecting the top two teams provides the output required by the Knockout Stage Simulation algorithm. 

Algorithm 4: Knockout Stage Simulation 

This algorithm is used to simulate the knockout stage after the group stage has been completed and the qualifying teams have been identified. The 16 qualifying teams are paired into 8 knockout matches, with the winner of each match progressing to the next round. The Match Simulation algorithm is reused to determine the winner of each fixture. The process is repeated for the quarter-finals, semi-finals and final, with the number of remaining teams being reduced after each round until one team remains. This is appropriate because knockout football requires a single winner to progress from each fixture, meaning that each match can be treated as an elimination process. Reusing the Match Simulation algorithm ensures that the same rating-based probability model and stochastic process used during the group stage are also applied to the knockout stage. This allows the complete tournament to be simulated using a consistent match model. 

PSEUDOCODE 

 

This algorithm is appropriate because the repeated structure of the knockout stage can be represented using a loop rather than creating separate logic for each round. Each iteration represents one knockout round, with losing teams removed and winners transferred to the next round. This allows the same process to handle the round of 16, quarter-finals, semi-finals and final without duplicating the algorithm. Calling the Match Simulation algorithm for every fixture also ensures that the probability model remains consistent throughout the tournament. The algorithm therefore connects the teams produced by the group-stage algorithms to the final tournament winner. 

Algorithm 5: CSV Data Validation 

This algorithm is used to validate the team data loaded from the CSV file before it is used by the simulator. Each row of the CSV file is checked to ensure that required values are present and that data is stored in the expected format. The team rating is checked to ensure that it is a valid numerical value, while team identifiers are checked to ensure that they are not duplicated. If invalid data is detected, the program reports an appropriate error rather than using the invalid data in the tournament. If the row passes all validation checks, it can be used to create a Team object. This is appropriate because invalid input data could cause incorrect tournament results or runtime errors, so validating the data before processing protects the reliability of the simulator. 

PSEUDOCODE 

 
 

This algorithm is appropriate because validation is performed before the data is used by the main tournament algorithms, preventing invalid data from being propagated through the system. Checking for blank values, duplicate identifiers, and invalid ratings addresses different types of input error that could affect the simulation. Reporting an error also allows the user to identify and correct the source of the problem rather than the program failing unexpectedly later in the tournament. Separating validation from the tournament algorithms allows the same validated team data to be safely used by the rest of the system. 

Algorithm 6: Results Export 

This algorithm is used to permanently store the results of a completed tournament by exporting the relevant match and tournament data to a CSV file. After the tournament has finished, the algorithm opens or creates the results.CSV file and writes the appropriate headings before processing each recorded match. The teams involved, match result and relevant tournament information are then written as individual rows. Once all results have been exported, the file is closed and the user is informed that the results have been successfully saved. This is appropriate because the results would otherwise only exist temporarily while the program is running and would be lost when the program closes. Exporting the results to CSV therefore provides permanent storage and allows the user to access the results after the simulation has finished. 

PSEUDOCODE 

 

This algorithm is appropriate because it systematically transfers the tournament data held in memory into permanent storage. Writing each match as a separate row allows the results to be stored in a structured format that can be accessed after the program has ended. CSV is also consistent with the input data storage used by the simulator, allowing the program to use the same simple file-based approach for both reading and writing data. The confirmation message provides feedback to the user that the export has completed successfully. 

D4 USABILITY FEATURES 

Terminal Interface 

The simulator will use a terminal-based interface to keep interaction simple and focused on the computational purpose of the system. The user will only be required to enter the information necessary to load the tournament data, start a simulation and view the results. A terminal interface is appropriate because the simulator is designed to explore tournament outcomes rather than act as a football game requiring complex visual controls. It also allows the same Python program to be run across the Windows computers used at school and Linux/macOS environments used during development. 

Input Validation 

Input validation will be used when the user selects or provides tournament data. The program will check that the CSV file can be accessed and that the required data is present and in the correct format before the simulation begins. This prevents invalid data from being passed to the tournament algorithms and reduces the likelihood of incorrect results or unexpected program errors. 

Clear Error Messages 

The simulator will provide clear, meaningful error messages when invalid input is detected. For example, if a CSV file is missing, cannot be read, contains duplicate team names or contains a non-numeric rating, the user will be told what is wrong rather than receiving an unexplained Python error. This allows the user to correct the problem without needing to understand the internal implementation of the program. 

Clear Output 

Tournament results will be presented using clearly labelled sections for match results, group standings and knockout-stage progression. Team names, scores and standings will be formatted consistently so that the user can identify the outcome of the simulation without needing to interpret raw program data. This is particularly important because the simulator produces a relatively large amount of information during a complete 32-team tournament. 

Progress and Status Messages 

The program will provide messages indicating the current stage of the simulation, such as when teams are being loaded, groups are being generated and different tournament rounds are being simulated. This provides feedback that the program is still processing the tournament and prevents the user from incorrectly assuming that the program has stopped responding. 

File Handling Messages 

The program will provide confirmation when team data has been successfully loaded and when tournament results have been successfully exported. If a file cannot be opened or saved, an appropriate error message will be displayed instead. This makes file handling visible to the user and reduces uncertainty about whether their data has been processed or saved. 

Repeat Simulation 

The interface will allow the user to run another simulation without having to restart the entire program. This is appropriate because repeated simulation is a central purpose of the system, allowing users to observe how different random outcomes can occur while the underlying team ratings remain unchanged. 

D5 Key Constructs 

The simulator will use object-oriented programming, lists, dictionaries, variables and CSV file structures as its main constructs. These constructs have been selected because they are appropriate for representing the teams, groups, matches and tournament progression while keeping the different responsibilities of the system separated. 

Team class 

The Team class will represent an individual football team. Its attributes will include values such as the team's name, identifier, rating, group and tournament statistics. Methods can be used to update statistics and access information about the team. Using a class means that each team's data is stored together rather than using separate variables for every team, making the simulator scalable to all 32 teams. 

 

Figure 3: Team class diagram 

Group class 

The Group class will represent one of the 8 groups in the tournament. It will contain a list of Team objects and the matches associated with that group. Methods will be used to generate or process the group's fixtures and calculate its standings. This separates group-level processing from the individual Team objects. 

 

Figure 4: Group class diagram 

Match Class 

The Match class will represent an individual fixture between two teams. It will store the participating teams, the match result and any required score information. The Match Simulation algorithm will operate on the data represented by this class. This allows each match to be stored and processed independently before its result is used by the group standings or knockout algorithms. 

 

Figure 5: Match class diagram 

Tournament class 

The Tournament class will manage the overall tournament structure. It will contain the groups, qualified teams, knockout rounds and final winner. Methods within this class will control the progression from the group stage through to the final and coordinate the other classes. This provides a central structure for the complete simulation without placing all tournament logic inside a single procedure. 

 

Figure 6: Tournament class diagram 

Lists 

Lists will be used to store collections of objects where the order or ability to iterate through the items is useful. For example, a group will contain a list of four Team objects, while the knockout stage can use a list containing the teams remaining in the current round. Lists are appropriate because the number of teams in each collection is known and the program needs to repeatedly process each team or match. 

Dictionaries 

Dictionaries will be used where data needs to be associated with a unique key. For example, a dictionary can store the number of tournament wins for each team when multiple simulations are performed, using the team name or identifier as the key. This allows the program to access and update a team's statistics efficiently without searching through an entire list. 

Variables & Data types 

Variables will be used to store temporary values during processing, such as probabilities, random values, points, scores, ratings, and loop counters. Appropriate data types will be used for these values. For example, team ratings and points will use numerical data, while team names and identifiers will use strings. Boolean values can be used for conditions such as whether data has passed validation or whether a team has qualified. 

CSV Files 

CSV files will provide permanent storage for team input data and exported tournament results. The input CSV will contain the required information for each team, including its rating. The program will read the data using Python's CSV functionality and validate it before creating the corresponding Team objects. Results can then be written to a separate CSV file so that completed simulations remain available after the program closes 

Validation 

Validation will be performed before externally supplied data is used by the main simulation. The program will check that required fields exist, team identifiers are unique, ratings contain valid numerical values, and the required number of teams is available. Invalid data will be rejected with an appropriate error message rather than being passed into the tournament algorithms. This protects the reliability of the system and prevents invalid input from affecting the results. 

Relationships between constructs 

The constructs work together as part of the complete solution. A Tournament contains Group objects, each Group contains Team objects, and the matches between those teams are represented by Match objects. The tournament algorithms use these objects to simulate matches, calculate standings, and progress through the knockout stages. CSV data is converted into Team objects after validation, while completed match and tournament data can be converted back into CSV format for permanent storage. 

 

Figure 7: Relationship Structure between constructs 

D6 Iterative Test Data 

During development, test data will be used repeatedly after individual components and algorithms are implemented. The test data will include normal, boundary, and erroneous cases so that individual components can be checked before being integrated into the complete simulator. This will allow errors to be identified close to where they are introduced and will reduce the difficulty of debugging the completed system. 

 

Test ID 

Component 

Test data 

Expected result 

Type 

I01 

CSV loading 

Valid CSV containing exactly 32 unique teams with valid numerical ratings 

All 32 teams are loaded successfully 

Normal 

I02 

CSV loading 

CSV containing fewer than 32 teams 

Program rejects the data and displays an appropriate error 

Boundary/Erroneous 

I03 

CSV validation 

Duplicate team name/identifier 

Duplicate is detected and data is rejected 

Erroneous 

I04 

CSV validation 

Team rating contains text such as "abc" 

Invalid rating is detected and reported 

Erroneous 

I05 

CSV validation 

Required column missing 

Program reports the missing column 

Erroneous 

I06 

Probability model 

Team A and Team B have identical ratings 

Both teams receive a probability of 0.5 

Boundary 

I07 

Probability model 

Team A has a substantially higher rating than Team B 

Team A receives a probability greater than 0.5 

Normal 

I08 

Probability model 

Team A has a substantially lower rating than Team B 

Team A receives a probability below 0.5 

Normal 

I09 

Match simulation 

Same two teams simulated repeatedly 

Different outcomes can occur, demonstrating randomness 

Normal 

I10 

Group generation 

Valid 32-team dataset 

Exactly 8 groups containing 4 teams each are produced 

Normal 

I11 

Group fixtures 

One group containing 4 teams 

Each unique pairing is generated exactly once 

Normal 

I12 

Group standings 

Predefined match results with known points and goal difference 

Teams are ranked according to the defined rules 

Normal 

I13 

Group standings 

Two teams have equal points 

Correct tie-breaking rule is applied 

Boundary 

I14 

Knockout stage 

16 valid qualifying teams 

8 fixtures are generated 

Normal 

I15 

Knockout progression 

Winners supplied for each knockout round 

16 → 8 → 4 → 2 → 1 progression occurs correctly 

Normal 

I16 

Results export 

Completed tournament 

Results CSV is created containing the expected records 

Normal 

I17 

Results export 

Attempt to save to an invalid/unavailable location 

Clear error is displayed rather than an unhandled crash 

Erroneous 

 

These test cases have been selected to test both normal operation and situations that could cause the system to produce incorrect results. Boundary data is particularly important for the probability and tournament algorithms because values such as equal team ratings and equal standings can expose logical errors. Erroneous CSV data is also tested because the simulator depends on external input before the tournament can begin. The same tests can be repeated after changes to the code to ensure that previously working functionality has not been broken by later development. 

D7 Post-development Test Data 

After development is complete, additional test data will be used to test the complete integrated system rather than only individual components. This testing will include full tournament simulations, repeated simulations for statistical accuracy, invalid files, boundary cases, and usability testing. The purpose of this stage is to determine whether the completed system meets the requirements and success criteria rather than simply checking whether individual algorithms operate correctly. 

Test ID 

Test 

Test data / method 

Expected result 

Purpose 

P01 

Complete tournament 

Valid 32-team CSV 

Full tournament completes from group stage to final and produces exactly one winner 

Tests complete system 

P02 

Repeated tournaments 

Run 20 complete tournaments using the same valid data 

Every tournament completes without an error 

Tests reliability 

P03 

Statistical accuracy 

Run at least 100 tournaments using teams with deliberately different ratings 

Higher-rated teams win tournaments more frequently overall 

Tests probability model 

P04 

Equal ratings 

Give all teams the same rating and run many simulations 

No team consistently dominates due to its rating 

Tests probability fairness 

P05 

Extreme rating difference 

Give one team a substantially higher rating than another 

Higher-rated team wins more often but is not guaranteed to win every match 

Tests non-deterministic behaviour 

P06 

Group standings 

Use a predefined set of match results with known points and goal difference 

Program produces the manually calculated ranking 

Tests tournament rules 

P07 

Tie-breaking 

Create teams with equal points but different goal differences 

Correct tie-breaking order is produced 

Tests boundary condition 

P08 

Invalid input 

Missing CSV, duplicate team, invalid rating and missing field 

Appropriate error messages are displayed and program does not crash 

Tests reliability 

P09 

Results persistence 

Complete tournament, export results, close program, reopen saved file 

Saved results match the original tournament results 

Tests permanent storage 

P10 

Cross-platform 

Run the completed program on school Windows machine and development environment 

Program runs and produces equivalent results 

Tests portability 

P11 

First-time user 

Give the completed program to a user without prior explanation 

User can load/run a tournament and understand the output without assistance 

Tests usability 

P12 

Large repeated simulation 

Run many tournaments for statistical testing 

Program completes within the defined performance requirement and produces usable results 

Tests performance 

 

I have selected these tests to test the completed system against the requirements rather than only checking individual algorithms. Repeated tournament simulations are particularly important because the main objective of the probability model is not that one specific team always wins, but that the distribution of results reflects the relative strength of the teams over a sufficiently large sample. Testing equal and substantially different ratings also checks that the probability model behaves appropriately at important boundary conditions. 

File persistence and cross-platform tests check the reliability of the completed software, while testing with a user who has not previously used the simulator provides evidence that the usability features work in practice. The results from these tests will be recorded and used during the Evaluation section to determine whether the final system has met its success criteria. 
