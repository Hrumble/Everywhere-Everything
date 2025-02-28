

>[!info] Who Are The Villagers?
>The villagers are your inhabitants and your population, they are the base of your economic and political systems. Each villager has multiple variables which dictates how he lives, and acts.

# Variables

## Name

The name of the villager, composed of its father’s last name, as well as a randomly chosen name.

## Age

The age of the villager starts at $0$ and goes up by one every year from the date of his birth. Once the age reaches a certain value $x$ the villager dies.

$x$ is defined by the following formula:

$x = y + randint(-10, 10)$

Where $y$ is the [[#Life Expectancy]] of the villager

The villager can also die if his [[#Hunger]] or [[#Sickness]] reaches $100$%

## Life Expectancy

A random number decided at birth between 50 and 80

## Sickness

Sickness is a number that starts at 0% on birth, and that has a random chance to get to 1% at a random point in the villagers life. Once the sickness is above 0% it will keep on increasing day by day until the villager gets healed.

## Hunger

Hunger starts at 0% and goes up every tick by 1%. To be reduced, the villager must feed himself according to his [[#Food Requirements]]. His hunger will drop by how much food he is being fed x based on how much food he needs y according to the following formula:$$hunger = hunger - 100 \times\frac{x}{y} $$
## Food Requirements

Food requirements are calculated at the end of each day based on a multitude of factors. Each action taken during the day is added to the food requirement according to the following formula:
$$k + (2400 - k) \times e^{-\theta}$$
Where $k$ is the amount of energy a particular action takes, and $$\theta = \frac{(x-\beta)^2}{200}$$
where
- $x$ is the age of the villager
- $\beta$ is the mean (age at which the food requirements peak)

>[!info] $\beta$ is a random number between $30$ and $50$

