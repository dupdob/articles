# About state
This series of post will try to give a tour d'horizon of the concept of 'state' in programming as well as a set of tools and approaches that you be useful to you as they were to me.
Expressing in an otherway, those are a braindump of my experience and pas reflections as a somewhat succesful multithreading developper, posted so that they may be of some help to others.

# What is a state?
According to wikipedia, 'state' in computer science is derived from the notion of stateful systems, i.e. it is all the system's _remembered information_. Then there is a reference to _finite_state_machines_. This is great, but a this is a C.S. definition. One can find it hard to translate it to a coding activity.
As far as I am concerned, I use the term 'state' to describe a consistant set of data, i.e. those data must respect some rule(s) as a group.
Let us use a simplified home address as an example: it will typically constain some integer for the street number, a string for the street name, another integer for the ZIP code and a string for the city name. All those fields must adhere to several rules:
- the ZIP code must be compatible with the city
- the street must exist within the city
- the street number must exist within the street
And typically, the home address changes as a whole when one moves to a new home. If by any chance, a single field was changed in isolation, let's say the city name, this is no longer a valid address: the street name and the ZIP code are probably invalid for this city.

Note that this description appears to have very little to do with our computer science definition: where is the state machine here? On the other hand, we see also an important actor: we need an obervator that will confirm the consistence of the data.