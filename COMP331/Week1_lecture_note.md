# COMP 331 Note

## Week1 Lecture Note

##### Tools: Gurobi Optimizer

### Course Intro Problem 1: Mortimer Middleman

**Problem Description:**

Motimer middleman is a diamond salesman, who is living depands sale diamonds, for **\$900/carat** .If they run out of diamonds, they travel to Antwerp and buy a certain carats of diamonds. They will pay **\$700/carat** to buy new diamonds. And whenever they place an order in Antwerp, they have to order **at least 100 carats** diamonds. And then they go back, and they have restocked their supply and restart selling diamonds. The cost of traveling to Antwerp is **\$2000**(money cost) for **1 week**(time cost). The Middleman could sales **55 carats per week** on average. We want to figure out which point Mortimer should place a new order(when the stocks touch the threshold). Besides, they have to pay **\$ 3.50/carat each week** for holding diamonds in their warehouse or banks.

**_Decisions:_** 

1. Reorder point --At which level(thersolds) of stock does mortimer go to Antwerp and make a place in new order.

2. Reoreder quantity --How much diamonds should they reorder.

**_Constraints:_**

1. Reorder point $\geq$ 0

2. Reorder Quantity $\geq$ 100

**_Objectives:_**
$minimize\ cost\ c(q,r)$

Some diagram to illustrate the mathematical model:


<figure>
    <img src="images\w1p1.png" alt="With Safety Stock" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;"> [Fig.1] With Safety Stock</figcaption>
</figure>


<figure>
    <img src="images\w1p2.png" alt="No Safety Stock & Loss Sales" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;">[Fig.2] No Safety Stock & Loss Sales </figcaption>
</figure>

<figure>
    <img src="images\w1p3.png" alt="With Loss Sales" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;">[Fig.3] With Loss Sales </figcaption>
</figure>

#### $Closed-Form\ Solution\ for\ the\ General\ Case$

$r := reorder\ point$
$q := reorder\ quantity$
$d := weekly\ demand$
$f := cost\ of\ replenishment$
$h := cost\ per\ carat\ per\ week\ for\ holding$
$l := lead\ time\ for\ replenishment$
$m := minimum\ order\ size$

$Sol：$

$
firstly\ the\ no\ loss\ sales\ period\ is:\\
\frac{sales\ price - cost\ price}{storge\ cost} = \frac{900-700}{3.5} \approx 57.14\\
that\ means\ 1\ carat\ diamond\ will\ make\ loss\ at\ 57.14\ week\ after\ buy\ in.\\
The\ reorder\ point\ should\ be\ 55,\ that\ means\ when\ the\ sales\ man\ get\ back\ from\ Antwerp,\ the\ stock\ goes\ to\ 0,\ and\ we\ won't\ run\ out\ of\ diamonds\ theoretically
$

By these analysis, we can got some values and constrains:
$
r \geq 55\\
q \geq 100\\
The\ cost\ could\ be\ divided\ into\ 2\ part:\ Travel\ cost\ and\ Storage\ cost.
The\ First\ part\ is\ travel\ cost.\\
The\ times\ of\ travel\ is\ \frac{q}{55}\\
it's\ inverse\ could\ be\ considered\ as\ how\ many\ times\ should\ salesman\ travel.\\
So,\ we\ could\ use\ that\ to\ calculate\ travel\ cost\ per\ week.\\
Thus,\ the\ travel\ cost\ per\ week\ is:\\
c(r,q)_{Travel} = \frac{55}{q}2000\\
Now\ let's\ going\ to\ holding\ cost:\\
The\ safety\ stock\ is:\ r-55\\
Because\ the\ procedure\ is\ linear,\ thus,\ we\ could\ use\ a\ simple\ average\ method:\ average = \frac{maxmum - minimum}{2}\\
Thus,\ we\ got\ the\ holding\ cost\ per\ week：\\
c(r,q)_{holding} = 3.5 * (\frac{(r - 55 + q) + (r - 55)}{2})
$

Organize the above formulas, we are able to constrate a optimization problem:

$
minimise:\ c(r,q) = c(r,q)_{travel} + c(r,q)_{holding} = \frac{55}{q}2000 + 3.5 * (\frac{(r - 55 + q) + (r - 55)}{2})\\
s.t.\ r^* = 55\\
\ \ \ \ \ \ q \geq 100\\
substituting\ r^* = 55\ into\ objection,\ we\ got:\\
c(q) = \frac{55}{q}2000 + 3.5 \frac{q}{2}\\
calculate\ the\ gradient:\\
\frac{\partial c(q)}{\partial q} = -55*2000*q^2 + 3.5*\frac{1}{2}\\
Let\ the\ gradient\ equals\ to\ 0,\ we\ got:\\
q^* \approx \pm 250\\
due\ to\ the\ constrains\ q^* = 250\\
c^* = 565\\
So,\ we\ got\ the\ optimal\ cost:\ \$ 565\ per\ week,\ and\ \$ 29380\ per\ year.
$


### Course Into Problem 2: Two Crude Petroleum
**Problem Description:**
Two Crude Petroleum runs a small refinery on the Texas coast. The refinery distills
crude petroleum from **two sources, Saudi Arabia and Venezuela**, into three main
products: **gasoline, jet fuel, and lubricants**.
The two crudes differ in chemical composition and thus yield different product
mixes. **Each barrel of Saudi crude yields 0.3 barrel of gasoline, 0.4 barrel of jet fuel, and
0.2 barrel of lubricants**. On the other hand, **each barrel of Venezuelan crude yields 0.4
barrel of gasoline but only 0.2 barrel of jet fuel and 0.3 barrel of lubricants**. The remain-
ing 10% of each barrel is lost to refining.
The crudes also differ in cost and availability. **Two Crude can purchase up to 9000
barrels per day from Saudi Arabia at \$100 per barrel**. **Up to 6000 barrels per day of
Venezuelan petroleum are also available at the lower cost of \$75 per barrel** because of
the shorter transportation distance.
Two Crude’s contracts with independent distributors require it to **produce 2000
barrels per day of gasoline, 1500 barrels per day of jet fuel, and 500 barrels per day of
lubricants**. How can these requirements be fulfilled most efficiently?

**Decisions:**
$x_1$ is barrel of Saudi crude(in 1000s).
$x_2$ is barrel of Venezuela crude(in 1000s).
$x_1,\ x_2\ \in \R$

| | gas | jet fuel | lube | costs |
|----------|----------|----------|----------|----------|
|Saudi:| 0.3 | 0.4 | 0.2 |100|
|Venezuela:| 0.4 | 0.2 | 0.3 | 75 |
|Demand:| 2 | 1.5 | 0.5 |

$Sol:$
$
min\ 100x_1+75x_2\\
s.t.:\ 0 \leq x_1 \leq 9\\
\ \ \ \ \ \ \ \ \ \ 0 \leq x_2 \leq 6\\
\ \ \ \ \ \ \ \ \ \ 3x_1 + 4x_2 \geq 20\\
\ \ \ \ \ \ \ \ \ \ 4x_1 + 2x_2 \geq 15\\
\ \ \ \ \ \ \ \ \ \ 2x_1 + 3x_2 \geq 5
$

<figure>
    <img src="images\w1l1.png" alt="2-Dimensional" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;">[Fig.4] 2 Dimensional Feasible Set </figcaption>
</figure>

If we intro the cost axis to show that data, we got:

<figure>
    <img src="images\w1L1_3D.png" alt="3 Dimensional Feasible Set" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;">[Fig.5] 3 Dimensional Feasible Set </figcaption>
</figure>

Then we can mapping z-axis on x-y plane:

<figure>
    <img src="images\w1L1_mapping.png" alt="mapped data" style="display: block; margin-left: auto; margin-right: auto;">
    <figcaption style="text-align: center; font-family: 'Times New Roman', Times, serif;">[Fig.6] mapped data</figcaption>
</figure>

This figure is something like equipotential surface in physics or contour line in geometry. Thus, we could get the answer easily, the target point is: The intersection point of the functions $0.3x_1 + 0.4x_2 = 2$ and $0.4x_1+0.2x_2 = 1.5$.

