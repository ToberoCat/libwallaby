# Increasing Robot Accuracy

<secondary-label ref="tobias-madlberger"/>

For precise movement on a game table, there's a key principle to remember: **The robot must always know its position**.
This
means the robot needs to be aware of its current location on the table and where it needs to go next. This is crucial
when the robot must reach a specific spot, such as to pick up an astronaut or drop off a pom.

This is easier said than done, especially since external reference points like GPS aren't allowed.

## X-Y Approach

The X-Y approach offers a straightforward method to ensure the robot consistently knows its x and y coordinates when
needed. This is achieved through various squaring-up techniques.

### Back-EMF

<tldr>
    <p><b>Summary</b>:Back-EMF is a method that measures motor ticks to determine the robot's position.</p>
    <p><b>Pros</b>: Can be done anywhere on the gametable.</p>
    <p><b>Cons</b>: Not reliable for long distances.</p>
</tldr>

[Back-EMF](https://en.wikipedia.org/wiki/Counter-electromotive_force) measures the ticks of the robot’s motors to
estimate its position. However, this method can be inaccurate, as
the measured ticks may deviate from the actual ticks, especially over longer distances. It works best for short
movements.

![back-emf-deviation.png](back-emf-deviation.png)

The graph above illustrates the deviation when using the Back-EMF method, showing significant inaccuracy over longer
distances. This data is from the
paperr [Enhancement of Accuracy in Botball Navigation](https://www.kipr.org/wp-content/uploads/2024/08/Enhancement_of_Accuracy_in_Botball_Navigation.pdf),
presented at GCER 2024.

### Aligning with the Walls

<tldr>
    <p><b>Summary</b>: Aligning the robot with the game table walls by driving against PVC pipes.</p>
    <p><b>Pros</b>: Simple to implement.</p>
    <p><b>Cons</b>: The robot must be at the table's edge.</p>
</tldr>

This method involves aligning the robot by gently driving it against the PVC pipes along the game table's edges. This
ensures that the robot's x or y position is accurate and can also correct its rotation.

ToDo: Add graphic of the X-Y approach with pvc alignment

### Line Up

<tldr>
    <p><b>Summary</b>: Aligning the robot with the black lines on the game table.</p>
    <p><b>Pros</b>: Can be done at black lines.</p>
    <p><b>Cons</b>: Slightly more complex algorithm.</p>
</tldr>

Similar to the wall alignment method, this approach uses line-follow sensors to align the robot with black lines on the
table. This method ensures the robot knows its x or y position.

To-Do: Include a graphic showing the X-Y approach using line-follow sensors.

For more details, check out this video by Jonathan Harrison on writing a line-up
method: [Square Ups](https://www.youtube.com/watch?v=jBVklWdPp7g)

## Line Following

<tldr>
    <p><b>Summary</b>:Following a line on the game table to determine the robot's position.</p>
    <p><b>Pros</b>: Simple way to navigate the robot on the game table.</p>
    <p><b>Cons</b>: Slightly more complex algorithm.</p>
</tldr>

Line following is another straightforward way to navigate the game table. The robot follows a black line to track its
position, which is especially useful when it needs to reach a specific spot.

Jonathan Harrison explains how to implement a line-following method in this
video: [Line Following](https://www.youtube.com/watch?v=i_qSt3hAxzc)

## Botball Analysis

To see the X-Y approach in action, I recommend watching [Botball Analysis](https://www.youtube.com/watch?v=OVaKQVpcXsI)
by Jonathan Harrison. In this video series,
Harrison analyzes various Botball runs, highlighting what was done well and what could be improved. It's a great
resource for anyone looking to enhance their Botball strategies, and I've learned a lot from it.

<include from="snippet-lib.md" element-id="footer">
    <var name="authors">Tobias Madlberger</var>
</include>