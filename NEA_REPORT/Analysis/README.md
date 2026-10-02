A1. Computational Methods 

Predicting a football tournament is usually done informally, through pundits, fans and bookmakers forming opinions based on reputation, previous performance and personal judgement rather than a transparent and repeatable method. However, the problem can be approached computationally because it involves repeatedly simulating match outcomes and applying a fixed set of tournament rules to determine how teams progress. Group standings are calculated using defined rules such as points and goal difference, while the knockout stage follows a fixed bracket structure. 

Tournament progression is therefore a deterministic rules engine once match results exist. Group standings can be calculated using fixed formulae, and knockout progression follows a defined sequence of rounds. These are examples of unambiguous logic that a program can execute consistently without the need for a person to manually track the results of 32 teams. 

Match outcomes are different because they are probabilistic rather than deterministic. A team's strength rating can be used to give it a greater or smaller probability of winning, after which a random process determines the result. This allows uncertainty to be represented explicitly in the program rather than relying on a subjective prediction. Using weighted random selection also means that the same teams can produce different tournament outcomes when the simulation is run again. 

The usefulness of the simulator comes from being able to repeat this process many times. Running one tournament only produces one possible outcome, whereas running the same tournament hundreds or thousands of times allows the results to be analyzed statistically. For example, a stronger team should generally win more often than a weaker team across a large number of simulations, while still occasionally losing. Automating these repeated simulations makes this analysis practical. 

The problem can also be decomposed into several well-defined sub-problems: loading and validating team data, creating groups, simulating group-stage matches, calculating standings, generating and simulating the knockout bracket, and storing results. Each sub-problem can be implemented and tested separately before being combined into the complete system. This makes the problem well suited to a modular computational solution. 

A2. Stakeholders and Users 

I have identified the following stakeholders and considered how each will use the simulator and why they are appropriate to the project. 

PE teacher 

Has knowledge of football and team performance, so will use the simulator by inspecting sample results and win-rate statistics to judge whether the outcomes appear reasonable given the input strength ratings. This stakeholder is appropriate because football knowledge is useful when assessing whether the simulated results are plausible within the context of the sport. 

Math teacher 

Has a statistical background and can assess whether the probability model is internally consistent. They will examine the relationship between team ratings and win probabilities and use repeated simulations to check whether stronger teams win more often over a sufficiently large sample. This stakeholder is appropriate because checking the statistical behavior of the model requires mathematical knowledge as well as general football knowledge. 

Friends who follow football 

These are realistic end users who can use the simulator to set up teams, run tournaments, and compare the results with their own expectations. They can provide feedback on both usability and whether the simulator produces outcomes that seem reasonable from a football perspective. They are appropriate because they represent the main target audience of football fans interested in exploring different possible tournament outcomes. 

General friends with no football knowledge 

These users will be asked to complete a defined task, such as starting a tournament and viewing the result, without prior explanation. This will test whether the interface and instructions can be understood without relying on existing football knowledge. They are appropriate because this can reveal usability problems that may not be noticed by users who already understand football terminology and tournament structures. 

A3. Investigation, Existing Solutions and Appropriate Usability Features 

I researched three categories of existing approaches to football tournament prediction and simulation. 

FIFA/EA Sports video games 

These provide a highly detailed and realistic football experience, including licensed teams and leagues and detailed player-level modelling. However, they are closed commercial products, so the underlying match-outcome calculations cannot be inspected or easily adjusted by the user. They are also primarily designed around playing or watching individual matches rather than repeatedly running the same tournament to study how the distribution of outcomes changes. This makes them unsuitable for a user who wants to understand the probability model behind the results. 

Prediction/pundit websites and apps 

These are generally quick and easy to use, but many present a prediction or set of predictions without allowing the user to repeatedly simulate the same tournament and examine how the outcome changes. A single prediction also does not demonstrate the difference between one possible outcome and the range of outcomes that could occur when randomness is included. This limits their usefulness for exploring the effect of probability and team strength. 

Spreadsheet bracket predictors 

These can be relatively transparent because their formulae can be inspected and changed. However, a spreadsheet may simply calculate or record one predicted outcome for each match rather than repeatedly generating random results. It may therefore lack a dedicated simulation process, automatic bracket progression and an easy way to run the same tournament many times and compare the results. 

How my system addresses these 

The three approaches provide useful features but do not combine them in the way required for this project. Video games provide detailed simulation but hide their underlying calculations, while prediction websites and simple spreadsheet predictors generally focus on producing a prediction rather than exploring repeated random outcomes. My system addresses this by providing a lightweight and transparent tournament simulator in which team strength ratings can be changed in the input data and the same tournament structure can be simulated repeatedly. This allows users to see how different results can occur even when the underlying team strengths remain the same. 

Appropriate usability features identified for the simulator 

Based on the strengths and weaknesses identified during my investigation, I have identified the following usability features. These are separate from the computational functionality described in A4: 

Sensible pre-filled defaults for team setup (for example, a ready-made 32-team CSV template with example ratings), so a first-time user can run a tournament without first having to create or research all of the input data. This also allows a non-football-fan tester to reach a working simulation within the two-minute usability target. 

A single, clearly labelled "Run Tournament" action and an equally clear "Run Again" action, because repeated simulation is a central purpose of the project. Re-running a tournament should therefore require very little additional interaction. 

Plain-language labelling that avoids unnecessary football jargon, while still using familiar tournament terms where appropriate. For example, stages such as "Round of 16" should be clearly labelled rather than relying on unexplained abbreviations. This supports the requirement that a user without football knowledge can understand the interface unaided. 

Visible progress feedback while a tournament is being simulated, so that the user knows the program is still running even if the simulation takes several seconds. This is a usability feature rather than a solution to a performance problem. 

Colour or highlight coding for standings and bracket results, so that qualification and progression can be identified quickly without requiring the user to interpret every value in a table. 

No installation of additional GUI frameworks, with Tkinter used for the interface if a GUI is implemented. This keeps the technical setup relatively simple and avoids unnecessary external dependencies. 

A4. Features of the Proposed Computational Solution 

I have identified the essential features below and explained why each is included. 

Team and rating data loading from CSV, with validation. This is the foundation of the simulation because valid team and rating data are required before groups and matches can be created. CSV is suitable because it is easy for a non-technical user, such as the PE teacher, to open and edit using spreadsheet software. This also allows the effect of changing team ratings to be explored without changing the program code. 

Random group draw. This is required to create the 32-team, 8-group tournament structure before the group-stage matches are simulated. The Python random module can be used to shuffle or select teams, with a controlled seed available during testing when reproducible results are required. 

Probability-weighted match simulation. This is the central computational feature of the project. Match outcomes will be generated using a weighted-random model based on the difference between the teams' strength ratings. A stronger team will therefore have a higher probability of winning, but the result will not be predetermined. This allows the effect of both team strength and randomness to be demonstrated. 

Group standings calculation (points and goal difference). This is required to determine which teams qualify from each group. The standings will be calculated from the simulated match results using the defined tournament rules. This provides a clear rules-based component that can be tested against a manually calculated example. 

Knockout bracket generation and simulation through to a final. This is required to produce the final tournament winner. The qualifying teams will be placed into the knockout bracket and each round will be simulated using the same match-outcome model, with the winners progressing until only one team remains. 

Results display (scores, tables and bracket progress). The results need to be displayed clearly so that the user can understand what happened during the tournament. This includes match scores, group standings and progression through the knockout rounds. 

Repeat simulation and results storage/export. Repeated simulation is required to demonstrate how tournament outcomes vary between runs. Results can also be stored or exported so that completed simulations can be reviewed later rather than being lost when the program closes. 

A5. Limitations of the Proposed Solution 

Simplified probability model : The MVP will use a single weighted-random model based on team rating difference rather than a more advanced statistical approach. More complex methods, such as Elo-based rating updates or Poisson-based scoreline modelling, would require additional assumptions and testing. Implementing one clearly explained model correctly is more appropriate for the project than attempting several advanced models without being able to test them thoroughly. These methods can therefore be considered as stretch goals. 

No dynamic in-tournament rating adjustment : Team ratings will remain fixed during a simulated tournament rather than changing after each result. A true Elo system would update ratings as results occur, but this is not necessary to demonstrate the main objective of the project, which is to show that team strength affects the probability of different outcomes. Adding dynamic rating changes would also increase the complexity of the model and the amount of testing required. 

No advanced GUI or animation in the MVP : The MVP will use either a command-line interface or a simple Tkinter interface rather than a fully animated bracket visualisation. This is a deliberate scope decision so that development time can be focused on the probability model, tournament rules and testing. A more polished interface can be added as a stretch goal if the core system is completed successfully. 

Team data quality depends on the input ratings supplied : Ratings will be supplied locally through the CSV file rather than being obtained from a live external data source. Therefore, the realism of the results depends partly on the quality of the ratings provided. This is separate from the internal accuracy of the probability model, which can be tested by checking whether stronger-rated teams win more often across repeated simulations. 

A6. Requirements 

Software requirements 

Python 3, including the standard random module for probability-weighted outcome generation; classes and OOP to model Team, Group, Match and Tournament entities; the csv module for reading team data and writing results; Tkinter for the GUI if implemented; and Git with a GitHub repository for version control. Development will take place across a personal MacBook and a school Windows PC, so using a shared version-controlled repository will keep the codebase consistent and provide a dated history of development. 

Hardware requirements 

Any laptop or desktop capable of running Python 3 and Tkinter. No specialist hardware is required, keeping the project self-contained and allowing the stakeholders identified in A2 to test the software on a suitable computer. 

Functional requirements 

(the system must): allow the user to load 32 teams with associated strength ratings from a CSV file; validate the loaded data, including checking that ratings are numeric, team names are unique and the file can be read; randomly assign teams into groups; simulate group-stage matches using the weighted-random probability model; calculate group standings using points and goal difference; determine the teams that qualify for the knockout stage; generate and simulate the knockout bracket through to a final; display match results, standings and bracket progression; allow the user to run the tournament again; and save or export completed tournament results for later viewing. 

Non-functional requirements: usability (a user with no prior explanation can understand the main interface and start a tournament within a couple of minutes); performance (a full tournament simulation completes within a few seconds rather than taking minutes); reliability (the program handles unexpected or invalid input without crashing); maintainability (code is organised into clearly separated classes and functions with sensible naming and comments where necessary); data handling (team and result data are validated before use, with appropriate error handling for missing or invalid files); and accuracy (the probability model behaves consistently, so a team with a substantially higher rating should win more often than a lower-rated team across many simulations). 

A7. Success Criteria 

# 

Objective 

Measurable success criterion 

Justification / how tested 

1 

Simulate a full 32-team tournament from groups to final 

The program runs from start to finish without an error and produces exactly one tournament winner. 

Directly tests the core simulation requirement in A6. The program will be run at least 20 times to check that a complete tournament is produced each time without crashing. 

2 

Reflect team strength in outcomes 

A substantially stronger team's win rate is consistently higher than that of a substantially weaker team across a large number of simulations. 

Directly tests the accuracy requirement and the main computational principle described in A1. At least 100 simulated tournaments will be run and the resulting win rates compared. 

3 

Provide a usable GUI 

A tester with no prior explanation can start a tournament and view the final result within 2 minutes without assistance. 

Directly tests the usability requirement using the non-football-fan tester identified in A2. The tester's attempt will be timed and any difficulties recorded. 

4 

Produce accurate group-stage standings 

The standings produced by the program correctly reflect points and goal difference for a known set of test match results. 

Tests the rules-engine logic described in A1 and A4. The program output will be compared against a manually calculated example. 

5 

Load team data reliably 

A missing, corrupted or invalid CSV file is rejected with a clear error message and does not cause the program to crash. 

Directly tests the file-handling and reliability requirements in A6. Deliberately invalid files will be supplied during testing. 

6 

Persist tournament results 

Results from a completed tournament can be saved and reopened correctly, with the reopened data matching the original saved results. 

Directly tests the results-storage requirement in A4 and A6. A result will be saved, the program closed and reopened, and the stored data compared with the original. 

Supporting Technical Design (OOP, Algorithms and Data Structures) 

Object-oriented design 

Core entities will be modelled as Python classes to keep the program organised and responsibilities separated: Team will store information such as name, rating and group; Group will contain teams and calculate standings from match results; Match will store the two participating teams and the simulated result; and Tournament will manage the group draw, group-stage simulation, knockout bracket and final results. Keeping match simulation separate from the calculation of group standings means that each class has a clear responsibility and individual components can be tested separately. 

Algorithms 

The core probability model will use weighted random selection. Each team's probability of winning a match will be determined from the difference between the two team ratings, with the higher-rated team receiving the greater probability. Python's random module will then be used to generate the outcome. The exact weighting formula will be defined during the Design stage so that its behaviour can be tested and justified. 

Group standings will be calculated by accumulating points and goal difference from the match results, followed by sorting the teams according to the relevant ranking rules. Knockout progression will use a bracket structure in which teams are paired for each round and the winners become the inputs for the following round. This process continues until one team remains. 

A Poisson-based scoreline model is being considered as a stretch goal. Research into football modelling has shown that Poisson-based methods can be used to model goals scored, including approaches based on historical attacking and defensive performance. However, the MVP will not depend on this method, allowing the core probability model and tournament logic to be completed and tested first. 

File handling 

Team data will be read from CSV at the start of a tournament using validated and error-checked file handling. Missing files, unreadable files and files with missing or invalid columns will be detected and reported with a clear error message rather than causing the program to crash. Completed tournament results will be written to a results file, such as CSV or JSON, and validated again when reloaded. 

Data structures 

Teams will be represented as objects with named attributes such as name, rating and group rather than raw tuples, making the data easier to understand and manipulate. Groups will contain lists of Team objects. The knockout stage will use a list-based structure representing the teams remaining in each round, or a nested structure if this is more appropriate during the Design stage. Results from repeated simulations will be stored in a dictionary keyed by team name, with each value containing the number of tournament wins. This will allow win rates to be calculated efficiently for the statistical testing described in A7. 

Development Environment, Version Control and Deployment 

As with my other candidate project, development will take place across a personal MacBook and a school Windows PC, synchronised through a single shared GitHub repository using Git. SSH will be configured on the school machine so that the repository can be accessed without repeatedly entering credentials on shared hardware. This will maintain a consistent version history across both locations and provide dated commits showing the progression of the project throughout the NEA development period. 

Feasibility, Scope and Timeline 

Why achievable at A-level: the project uses techniques I already have practical experience with, including classes, dictionaries, file handling and the Python random module. It also introduces a small number of new but manageable concepts, such as probability-weighted outcomes and knockout-bracket logic. The project does not require external APIs or specialist hardware, and the GUI can be implemented using Tkinter if needed, which is included with Python. This keeps the technical scope realistic for the available development time. 

Essential (MVP): CSV team-data loading with validation, random group creation, group-stage simulation using a clearly explained weighted-random probability model, group standings calculation, knockout bracket simulation through to a final, and a basic results display using either the command line or a simple Tkinter interface. 

Stretch goals: Elo-based rating updates between rounds, more realistic scoreline distributions such as Poisson-based modelling, saved tournament history and aggregate statistics across multiple runs, and a more polished GUI with visual bracket graphics. 

Explicitly out of scope: any dependency on live external football data or APIs. All team and rating data will be supplied locally through CSV, keeping the project self-contained and avoiding external service dependencies during the NEA development period.
