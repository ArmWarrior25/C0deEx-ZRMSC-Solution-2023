# C0deEx-ZRMSC-Solution-2023
Zero Robotics is a unique and exciting program that combines space exploration, computer programming, and robotics education. It was originally initiated as a collaboration between the Massachusetts Institute of Technology (MIT) Space Systems Laboratory, the Defense Advanced Research Projects Agency (DARPA), and the National Aeronautics and Space Administration (NASA). The primary goal of Zero Robotics is to engage students in science, technology, engineering, and mathematics (STEM) fields through a competitive, space-themed programming competition. Zero Robotics main appeal is the abiltity to run unique codes on a custom MIT-IDE and the oppurtunity for top codes to run on experimental AstroBee and SPHERES satellites on the International Space Station.

The C0deEx Panthers is a veteran team in the Zero Robotics Summer Challenge. They won the competition in 2019 by being the only team to successfully carry out all tasks on the International Space Station and were the national runner-ups in the following SPYSPHERES and Spelling Bee Challenges. This repository includes the C0deEx Panthers full analysis and algorithmic approach to the 2023 Zero Robotics Middle School Summer Challenge.

For the 2023 LUNABEE challenge teams were tasked with identifying a set of active sites and then navigating the Astrobee optimally to collect a set number of samples while minimizing battery usage and time taken. The exact details are outline in the official game manuel below:

https://docs.google.com/document/d/13U_ueLhJu9CemtzKwgjeGeWMz1rSZFM21uFqQb854Lc/edit

Although this task may not sound to difficult, it was made challenging because of the specified collection and battery usage rules:

1. At each site the Astrobee can choose to collect, duplicate or both. Collecting adds the site number to their current number of samples while duplicating multiples the site number to their current number of samples. If the Astrobee gains an additional point for each sample collected and if it obtains and deposit exactly 24 samples, then get bonus points. On the other hand, if the Astrobee collects too many samples, it incurs collection penalities.

2. Battery usage is expressed as the $\sum Battery Usage Rate (BUR) \times distances$ over the run where distances represent the euclidean distance between sites and BUR is the rate the battery is used over a distance. The BUR is not constant is is modelled by $0.2 \times (1 + number of samples)$, thus to obtain the maximum score, the Astrobee should avoid carrying considerable amounts of samples over long distances.

There are other components: a time score which is optimized with shorter simulation lengths, time penalty which docks points for exceeding the time limit, and a battery score which docks points for using all the battery, but the two factors above are the most complicated and important for succeeding in the competition.

The net equation that can be used to evaluate the final score based on these subparameters is listed below:

![image](https://github.com/ArmWarrior25/C0deEx-ZRMSC-Solution/assets/87990660/320b4bce-bd8d-49b4-bdc8-a7c25d8bae57)

The C0deEx Panthers' unique approach to this challenge included developing three novel programs:

# compute_score

compute_score: This algorithm mimics the game manuel instructions to find the expected score for a given pathway. This pathways must be expressed in a string such as $+5*4+4$. This string codifies the process of moving to site 5 from homebase and collecting samples, moving to site 4 for duplication, and staying at site 4 to collect samples before returning home. Not only does this code return the expected scores, but also the expected battery, time, and collection subscores along with any expected bonuses or penalties. The battery and collection subscores along with penalties and bonuses are 100% accurate, but time score has significant fluctuations. This however produces minimal impact to the final score due to the low weightage of the time score. After 100+ tests, we have found this score predictor function to be accurate to about $2$ percent, and both serves a useful tool by itself and an essential subroutine for later codes. Not only can this function accurately display and predict final scores, it is also able to display each of the subscores, penalties, and bonuses if passed an additional boolean parameter.

# predictor

predictor: Given a set of active sites in the form of a string (ex. $2 4 5$) this function generates all possible pathways. Mathematically we can evaluate this total number of pathways considering at each active site by considering that at each site we can choose to duplicate, collect or do both. Thus the number of total pathways for a given set of active sites $a, b, c$, can be modelled as the number of ordered subsets of the string $+a \times a+b \times b+c \times c$. Basic combinatorics provides us with the total number of pathways being:

3. $\binom{6}{1} \times 1! + \binom{6}{2} \times 2! + \binom{6}{3} \times 3! + \binom{6}{4} \times 4! + \binom{6}{5} \times 5! + \binom{6}{6} \times 6! = 1956$

This total number of pathways is then trimmed to only include pathways with 24 samples or less. In our findings this reduces the number of pathways to 600 or less. Finally we evaluate compute_scores for each of the remaining pathways, and take the pathway from the max total score from this set. The function finally returns three outputs: the original set of active sites, the optimal pathway, and the expect score.

# createZRCode

createZRCode: This algorithm takes in a set of active sites. Then using the previously defined predictor function it returns the optimal pathway and prints the set of java scripts compatible with the Zero Robotics. These scripts can then be directly copied from the code and then implemented into the IDE text editor to run simulations.
