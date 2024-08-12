# Making a Successful Strategy

<primary-label ref="personal-experience"/>
<secondary-label ref="tobias-madlberger"/>

> This guide must not be the best guide to make a successful strategy,
> but it helped my team make one that got us to the fourth place at the 2024 GCER.
> It heavily relies on my personal experience and the experience of my team.
> {style="warning"}

> WIP: This guide is still a work in progress. I will add more content in the future.

## What is a Successful Strategy?

A good strategy isn’t just about how many points you score at the end of a competition. It’s also about how well you use
the resources you have, how much time you have to prepare, and the team
you’re working with, both in terms of experience and size.

For example, if you have a team of six experienced people and three months to prepare, you can aim for a high-scoring
run. But if your team has only two people who are less experienced, it’s better to aim for a run that’s easier to
execute and has high accuracy. In this case, consistently scoring 50 points is already a success.

## Accuracy Over Points

In my experience, it’s better to aim for a strategy that’s reliable in scoring the points you plan for, rather than
going for a high-scoring strategy that isn’t reliable. A dependable robot is much easier and less stressful to work with
during a competition than one that can score a lot of points but might fail when it counts.

Let me give you an example from GCER 2024. My team had a strategy that could score 2,700 points in theory, but at the
competition, we only managed to score 1,100 points. The issue was that our high-scoring strategy wasn’t reliable. Even a
small mistake cost us a lot of points, and unfortunately, our robot made more than one mistake. The reason we ended up
in fourth place wasn’t because we could achieve a 2,700-point run, but because we were able to consistently score around
1,100 points.

This experience taught me that being able to consistently achieve a lower, but reliable score is often more valuable
than risking everything for a higher, but unpredictable score. As a side note, the reliability of our robot made GCER
much more enjoyable for us, as we didn’t have to worry about our robot failing during the competition.

## Creating the Initial Strategy

The day before the game review was released, my team and I had a Discord call to discuss our goals for the year and how
much time each of us could commit to the project. To clarify, we were all students, and Botball wasn’t a school
subject—we had to work on it over the weekends.

With a general idea of what we wanted to achieve, we started looking at the game review as soon as it was released. We
studied the scoresheet and the game table, trying to identify which areas could score the most points. At this stage, we
weren’t thinking about what our team could realistically do, but rather about what was possible. We asked ourselves
questions like, “What can be done?” and “Which items score well together?” without worrying about risks or how the robot
would accomplish specific tasks. This approach is important because it helps you understand what the game allows and
what other teams might also be aiming for. We also asked questions to the KIPR staff to clarify the scoring rules. This
whole process took us about 3-4 hours of discussions and questions.

Once we had a solid understanding of what was possible in the game, we started brainstorming what our robot should be
able to do. We used a whiteboard session (in our case, the free tool [excalidraw.com](https://excalidraw.com/)) to
sketch out possible mechanisms, our robot design, and the tasks we wanted to accomplish. We also discussed the order in
which the tasks should be done and how the robot should be built to handle them. We even took a closer look at the new
Create3 robot to see how we could use it.

![Whiteboard Brainstorm Session](2024-botball-strategy-brainstorm.png)

> Here’s a picture of our whiteboard. I recommend opening it in a new tab to see it in full resolution.

As you can see in the picture, we mapped out the driving paths for the robot on the game table and sketched possible
mechanisms, like a pom sorting system and a creative astronaut pickup mechanism. We also drew some robot designs,
although we didn’t end up using all of them. The image also includes some features of the Create3 robot and screenshots
of our Excel scoring sheet.

## Refining the Strategy

After the brainstorming session, we had an initial idea of what our robot should be able to do. The next steps where a
little bit more diverged between the team members. Some of us started to build our concept robot, while the others
experimented with the Create3 robot and others tried t come up with more detailed movement strategies of the robot.

To focus on the refinement part of the strategy, I will talk about the movement strategy. When you want you robot to be
moving accurately on the game table, there's a simple rule you should follow: **The robot should always know where it
is.** This means that the robot should always know its position on the game table and where it has to go next. This is
especially important when you have to move the robot to a specific position on the game table, like when you have to
pick up an astronaut or when you have to drop a pom.

Achieving this is easier said than done. In our case, we are not allowed to use any external reference points, like gps
or other methods. This means that the robot has to rely on its own sensors to know where it is. A very good approach to
this is to make sure the robot knows where it is at the beginning of the run. This can be done by always making it start
in it's starting
position, which can easily be almost perfectly placed by a human in the setup time.

## X-Y Approach

With this set, I want to introduce the X-Y approach. This approach is a simple way to check when the robot can be sure
where it is. The idea is to have the robot always know its x and y position when needed. This can be done by using
various methods.

One not so reliable method is to use [Back-EMF](https://en.wikipedia.org/wiki/Counter-electromotive_force) of the
motors. This method is not reliable because the measured ticks can be
off by a few ticks from the real ones, which can lead to a wrong position of the robot. It is still good enough when
only small distances
are
needed ([Enhancement_of_Accuracy_in_Botball_Navigation](https://www.kipr.org/wp-content/uploads/2024/08/Enhancement_of_Accuracy_in_Botball_Navigation.pdf))

Another approach would be to align the robot with the walls of the game table by driving against the pvc pipes. This
will make sure the robot has the same x or y position. When done correctly, one can also fix the robot's rotation.

ToDo: Add graphic of the X-Y approach with pvc alignment

An approach similar to the one above is to use the line follow sensors to align the robot with the black lines. A line-up
like this allows the robot to know its x or y position.



<include from="snippet-lib.md" element-id="footer">
    <var name="authors">Tobias Madlberger</var>
</include>