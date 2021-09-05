---
layout: post
title:  "Calcium Carbonate to store CO₂ ? "
date:   2021-07-01 00:00:00
categories: climate 
---

The earth's oceans contain a great deal of dissolved inorganic carbon (DIC) and a decent concentration of calcium (about 10mM).
Furthermore the ocean has a huge surface area and is able to take up large quantities of CO₂ from the atmosphere.
It is thus sometimes suggested that inducing precipitation of calcium carbonate (CaCO₃) from seawater could be a viable method to 
lock down carbon into a solid form, which could then be safely and easily stored somewhere. The thought is that removing carbonate from the 
ocean would subsequently cause it to absorb more CO₂ from the atmosphere.

Unintuitively, it turns out that this is not the case due to the way in which the ocean carbon chemistry works. 
Paradoxically, the exact opposite is true - precipitation of CaCO₃ in the ocean **releases** CO₂ to the atmosphere - how can this be ? 
This fallacy appears frequently enough, surprisingly even in peer reviewed papers sometimes, that it bears illuminating. 

## Inorganic carbon in the ocean

Most gases like O₂ or N₂ are not all that soluble in water because water is a polar liquid and these small molecules are non-polar.
At first glance CO₂ might be thought to behave similarly, after all it's not particularly polar.
However, when CO₂ dissolves in water it can react with a hydroxyl ion to form a bicarbonate ion.

```
CO₂ + OH⁻  → HCO₃⁻ 
```

The bicarbonate ion can further react with another hydroxyl ion to form a carbonate ion.

```
HCO₃⁻ + OH⁻  → CO₃²⁻ 
```

<sup>(These reactions are often written as between CO₂ and water, releasing protons, but for 
illustration purposes i think it's more instructive to think of it the above way.)
</sup>

THe key is that unlike CO₂ these carbonate ions are charged and remendously soluble in water.

In pure water the availability of hydroxyl ions is very small, available only from the small amount of water that is always dissociated 
into H⁺ and OH⁻. Any CO₂ that dissolves from the air quickly reacts with the available OH⁻ and the subsequent imbalance of H⁺ and OH⁻ results in the solution to become acidic. 
This makes the availablity of OH⁻ even smaller and opposes further reaction.
Thus the solubility of CO₂ in pure water is not much greater than that of other simple gases like O₂ or N₂, which do not react with water.
How could we increase the ability of water to absorb CO₂ ? Simple: supply more OH⁻ ions that can gobble up CO₂ and 
turn it into bicarbonate, in other words, make the water more *alkaline*. This excess quantity of hydroxyl ions, which can react with CO₂, 
is called the water's "alkalinity". Thus the most basic way to express a quantifiable alkalinity is 

```
Alk = [HCO₃⁻] + 2×[CO₃²⁻] + [OH⁻] + ... 
```

This reflects both the excess OH⁻ that has already reacted with CO₂ into bicarbonate/carbonate ions (first two terms) and any free OH⁻ that is still available to react (third term).
This quantity effectively respresents the capacity of a body of water to absorb CO₂, irrespective of how much CO₂ it has currently taken up.
[Alkalinity](https://en.wikipedia.org/wiki/Alkalinity) is a complicated concept and a full expression includes a number of other weak bases, 
but the carbonate ions are typically the most important and this simplified view is sufficient to understand carbonate precipitation.

Seawater contains a large quantity of alkalinity, which originates from dissolving alkaline rocks, which flows into the sea with the rivers.
This alkalinity allows seawater to hold a huge quantity of CO₂; in fact the total quantity of CO₂ dissolved in the ocean (37000GtC) is 
45 times greater than in the atmosphere (840GtC). Most of it is, of course, in the form of bicarbonate (89%)  and carbonate ions (10%).
Only a tiny fraction (0.5%) is dissolved as actual CO₂, like an ordinary gas.  

## Precipitating carbonates 

To understand why precipitating carbonates lead to the release of CO₂ into the atmosphere, let's look at the precipitation of CaCO₃. 
The reaction can be written in a number of ways, but they all result in the same outcome:
Lets try one line of argument: Since most of the available carbonates are in the bicarbonate form (HCO₃⁻) one way to write the precipitation is : 

```
 Ca²⁺ + 2HCO₃⁻ → CaCO₃ + CO₂ + H₂O
```

You can immediately see that for every CaCO₃ created, one CO₂ is moved from the carbonate pool (highly soluble) to the gas form (poorly soluble),
and is thus released back into the atmosphere.

Another way to think about it is to ask "what would we need to do to restore the ocean to its prior state after precipitating a CaCO₃ ?"
First, we precipitate the CaCO₃:

```
 Ca²⁺ + CO₃²⁻ → CaCO₃ 
```

Now, let's restore the used-up CO₃²⁻ by bringing in a CO₂ from the atmosphere:

```
 CO₂ + 2OH⁻ → CO₃²⁻ + H₂O 
```

You can see that doing so requires us to add two OH⁻ back to the ocean, in addition to the CO₂. (We won't worry here about changes in the Ca²⁺ concentration). 
In other words a process that removes calcium carbonate from the ocean in a pH-neutral way needs to replenish the 
alkalinity from somewhere. If not, we will decrease the oceans capacity to store CO₂ by *two* units (we used up two OH⁻), for every *one* unit carbon we've precipitated.
Since we've only removed one mol of carbon from the ocean, another unit of carbon will be released to the atmosphere. 


## Making alkalinity

Ok, but what if we generated new OH⁻ (i.e. alkalinity) in some way (for example using electrochemistry, like 
the [Chlor Alkali](https://en.wikipedia.org/wiki/Chloralkali_process) process or by mining alkaline minerals) - could we use those to precipitate CaCO₃ and lock down CO₂ ?
Yes, absolutely that would work. For every two mols of OH⁻ you could lock down one mol of carbon:

```
 Ca²⁺ + CO₂ + 2OH⁻ → CaCO₃ + H₂O
```

But, wait! There's an even better thing you could do. You could simply add the alkalinity to the ocean and *not* precipitate any calcium carbonate. As we discussed earlier that would
raise the oceans capacity to absorb CO₂ by two mols! Now you're getting twice the bang for your buck: every mol of OH⁻ pulls down one mol of carbon! (actually the true number is a little lower, about 0.85 or so, for complicated reasons we won't go into here).
Why ? Because in the ocean most of the carbon is stored as bicarbonate instead of carbonate, and to convert a CO₂ to HCO₃⁻ you only need *one* OH⁻:


```
 CO₂ + OH⁻ → HCO₃⁻ 
```

Note that storing CO₂ in the ocean this way doesn't acidify the ocean, since we have added the necessary alkalinity to go with it.
There are many research proposals out there looking at the various ways to add alkalinity to the ocean to counteract it's acidification and provide capacity to store the excess CO₂ in a pH neutral way. To learn more, this is a good place to start: [Assessing ocean alkalinity for carbon sequestration](https://agupubs.onlinelibrary.wiley.com/doi/10.1002/2016RG000533)












