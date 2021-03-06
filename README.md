# BayesianNetwork-EpidemicModeling

\section{Introduction}
In the attached notebook, I developed a highly simplified Bayesian network to model the main consequences of an epidemic breakout in a nation, based on its population, density, GDP, health care quality, and restriction policies taken into account by that specific country.
The network is based on the famous SIR model. These variables (S, I, and R) represent the number of people in each compartment at a particular time:\\
S: percentage of susceptible individuals within the population.\\
I: percentage of infected individuals within the susceptibles.\\
R: percentage of removed individuals within the infected.\\
The last state of this model can be extended with recovered and deceased states. The model was designed considering the characteristics (infectivity, lethality) that have distinguished and still distinguish the COVID-19 pandemic.\\
At the end of the notebook there are some references from where I based the probabilities to obtain a reasonable model. Some of the data are impossible to find online so I managed to create some of them, trying to keep the model the most consistent as possible.

\section{The model}

The network follows the structure described in figure 1, based on the SIRD model. Sometimes, viruses do not behave in the same way for all the people of a country. Sometimes (e.g. Covid-19), it does not affect people under a certain age, other times a virus can not be effective on people that follow a specific diet. Moreover, in a large population, there is the possibility that a specific gene, in an immune system, is resistant to certain types of virus and it developed through the generations. Besides, the weather in a country can influence this parameter too.  Restriction Policies and Health Care system nodes are both influenced by the GDP. Indeed, strong economies (e.g. France) usually have a well-developed health care system, but it is not always true (e.g. India). 
\begin{wrapfigure}{r}{0.5\textwidth} 
    \centering
    \includegraphics[width=0.5\textwidth]{chart.png}
    \caption{Structure of the network}
    \label{fig:chart}
\end{wrapfigure}
Moreover, a greater GDP means that, during a pandemic, the country can invest more in medical tools such as masks, artificial lungs, painkillers, and so forth. Besides, the restriction policies are usually tougher when a country can afford them (e.g. Italy), but sometimes, rich countries could take the risks of the pandemic without efforts (e.g. UK in summer 2020). The node "Infected" is influenced by the density of the country. Indeed, in a very dense country (e.g. Bangladesh), people enter in contact with each other more frequently. Besides, the restriction policies limit the contact and the spreading of a virus. Finally, the number of recovered is influenced by the infectious and quality of the health care system. An efficient HC system can handle and cure a huge number of infected. On the other hand, countries with bad HC systems and where the population is old and fragile will record more losses.

\section{Network Analysis}
In this section of the notebook I use the functions provided by the library \textit{pgmpy} to investigate the network and show the main components of it:
\begin{itemize}
    \item The \textbf{leaves} (terminal nodes, thus without children) and \textbf{root} nodes (nodes without parents)
    \item \textbf{d-separation principle}: principle that determines whether a set $X$ of variables is independent from another set $Y$, given a third set $Z$. 
    \item \textbf{Active trial}: If influence can flow from X to Y via Z the trial $X\leftrightarrow Z \leftrightarrow Y$ is active. So, if there's no \textit{active trial} between two variables $X\ \&\ Y$ they are d-separated.
    \item \textbf{Markov Blanket}: The set of \textit{parents, children, and children's parents} of a node $\rightarrow$ the Markov Blanket is the minimal set of nodes which d-separates a particular node from all the others
\end{itemize}

This investigation is performed firstly on some single nodes and then globally on the network. All the functions provided by the library are enough to compute a good analysis. Although, I provided a function that searches if there exist an active trial between a list of given nodes and, if there's none, suggests a list of nodes that may be consider to find an active trial.\\
Other two interesting functions are provided, but they are just a refitting of some function from the official \textit{pgmpy} tutorials. They return the nodes that appear the most and the least in independence assertion as independent variable or given specific evidences.

\section{Inference}
In this section are explained the differences between \textit{exact inference} and \textit{approximate inference}.
Regarding the \textbf{exact inference} subsection, I built some evidence with some countries (based on the data provided by the resources) and provided some functions.
These functions return the table of infection, death, or recovered given a specific country. Moreover, there is another function, which uses a \textit{map query}, that returns the countries that have the most likely characteristics given the characters given in input. For example, (within the evidence) the countries more likely to have a low recovering rate are those countries that have a medium population, low GDP, low density, and so on (see notebook), and, from the evidence, Mongolia reflects these characteristics. Regarding the \textbf{approximate inference}, the subsection begins explaining the main techniques we encountered during the course. I provided a function that allows for making comparisons between these three methods. This function takes in input different values, as the number of samples, the variable and conditions of it (if the variable has to be $==, \leq or \geq $ than the desired value), and the evidence. I perform two examples for this function, computing  $P(RestrictionPolicies=Light|HealthCare=Weak, Decease=Low)$ and  $P(RestrictionPolicies\geq Light|HealthCare=Weak, Decease=Low)$. Then I performed a graphical comparison between the approximate inference techniques and the exact inference. This shows that the more samples we use the higher is the accuracy of the approximate sampling techniques. Thus, they converge into the exact inference value.

\end{document}
