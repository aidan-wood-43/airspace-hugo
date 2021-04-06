+++
author = "Aidan Wood"
bg_image = "/images/3-single-class-layout.JPG"
categories = ["Regulatory"]
date = 2021-03-05T00:00:00Z
description = "How we developed a simulation for BWB emergency egress."
image = "/images/econ2-s.jpg"
tags = ["Simulation", "Regulatory", "Emergency Egress", "Bwb"]
title = "Modelling Emergency Passenger Egress"
type = "post"

+++
Commercial aviation is incredibly safe - you’re more likely to die from a bee sting than to be in a plane crash - and the main reason for this safety is the intense scrutiny of all aspects of aircraft design by the global aviation industry. Organisations such as EASA and the FAA place strict safety requirements on all aspects of aircraft design, and proving an aircraft meets these requirements is a lengthy process including many physical tests.

Emergency egress is the process of evacuating all passengers in the event of an aircraft accident. EASA’s CS-25 document for the certification of large aircraft dictates that all passengers must be able to evacuate an aircraft in under 90 seconds while using no more than 50% of the available exits. For new conventional aircraft, this is carried out through full scale live evacuation simulations: in 2006, Airbus carried out a full scale evacuation of their 853 seat A380 to achieve certification. They evacuated everyone using 8 exits in only 78 seconds \[1\].

Ensuring all passengers are in close proximity to an exit becomes more difficult with a Blended Wing design, due to the higher volume to surface ratio. Realistic full scale evacuation simulations are expensive and difficult to organise: to-date, the only published record of a BWB evacuation was a partial BWB cabin evacuation conducted by the University of Greenwich at Cranfield University’s evacuation test centre \[2\]. This partial evacuation aimed to validate a computer simulation used to model a 1000+ seat BWB evacuation procedure, which suggested the design could be completely evacuated in under 90 seconds. However, this study failed to account for the difficulties faced when designing emergency exits for a BWB design, where aft and over-wing exits are difficult to implement. To answer the question of whether a 600 seat BWB design could be evacuated in under 90 seconds, using only fore emergency exits, a numerical simulation was developed in Python and validated against the 2006 A380 evacuation test.

The simulation revolved around three interconnected models:

1. Aircraft plan
2. Occupancy map
3. Graph network

![](/images/black-n-white.png)

_Visualisation of a typical Graph Network._

The aircraft plan was used to define the structure of the aircraft: the walls of the cabin, galleys, aisles, and seats. The floorplan informed the occupancy map, which was used to track occupants movements throughout the simulation. Finally, the graph network was devised to model the relations between each element of the spatially discretized domain, and to allow individuals to find realistic and logical paths to an emergency exit. This was achieved by modelling each point in the domain as a node in the network, with the weights of edges between nodes being informed by the topology of the floorplan: edges connecting to walls having very high weighting compared to edges connecting aisles. In this way, path finding algorithms such as Dijkstra’s algorithm could be used to find the paths of least weight connecting an individual node to an emergency exit node.

[View the GitHub Repo.](https://github.com/aidan-wood-43/bwb-evac-sim "GitHub Repo.")

Validation was completed by applying the model to an 868 seat A380. Considering the average of 10 runs, evacuation was completed in 73 seconds - 5 seconds less than Airbus’ 2006 test. However, this was deemed suitable to suggest an accurate model, and the simulation was then applied to a single class, 604 seat BWB design. With all exits available, evacuation was completed in 55 seconds on average. With only fore aircraft exits available, this time increased to 98 seconds.

![](/images/evac-bwb3.gif)

_BWB evacuation simulation_

Although the final result was slightly above the 90 seconds requirement imposed by CS-25, this was a worst-case evacuation scenario. Additionally, applying the model to the BWB design highlighted issues in the passenger exit logic implemented in the simulation where passengers are very hesitant to travel to distant exits. This means that passengers tend to ignore the quiet exits at the nose of the plane in favour of busier exits closer to them. This path finding logic affects the BWB design significantly more than the A380 because passengers are on average more likely to be far away from more exits when the aft exits are unavailable.

Considering this, it is likely that even the basic single class cabin design simulated would successfully evacuate in the required 90 seconds, however further work, including full scale testing, would be required to confirm this. Intermediate work should aim to redevelop the simulation pathfinding logic to allow passengers to make more informed decisions based on the business of exits, as they would in a real evacuation scenario.

Through this work, we have confirmed that aft exit placement does cause issues for BWB evacuation, but that these issues are not insurmountable. However, before any feasible BWB designs can be produced it is necessary to back up these findings by performing expensive and time consuming full-scale simulations. For now, however, numerical modelling such as this allows the BWB concept to develop further.

References

\[1\] [Business Insider. A380 full-scale evacuation.](https://www.businessinsider.com/time-escape-crashed-airliner-2016-8?r=US&IR=T "Business Insider. A380 full-scale evacuation.")

\[2\] [E.R. Galea et.al. (2010)](https://www.researchgate.net/publication/228824877_Evacuation_Analysis_of_1000_Seat_Blended_Wing_Body_Aircraft_Configurations_Computer_Simulations_and_Full-scale_Evacuation_Experiment "E.R. Galea et.al. (2010)")