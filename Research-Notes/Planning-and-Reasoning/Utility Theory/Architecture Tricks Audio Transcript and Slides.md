# Architecture Tricks Audio Transcript And Slides

Audio file

GDC2013-Mark.mp3

Transcript

Infinite access utility system. The name kind of spilled out of some
work with a couple of my clients consulting clients over the past couple
years as we just kept referring. To it as this and it\'s something I
have been accused of being excited about utility systems in the past,
but this one is really something that I\'m starting to say, the
culmination of kind of things I\'ve been working on for the past few
years. Very briefly, for those who don\'t know me, by now, my name is
Dave Mark and the President and lead designer of intrinsic algorithm
started as an independent studio. I\'ve been doing AI. For studios
small, medium and large, I\'m also the author of the book Behavioral
Mathematics for Game AI, and I\'ve written for a number of other
publications and I\'m a occasional writer for game Developer magazine as
well. Real Real 1 slide brief review of what is utility utility based AI
is really just using numbers and formulas and scores to rate the
possible benefit of an action a character could take. It can be used
with other architectures like with selectors and behavior trees or edge
weights in your planner. So somewhat like what Luke was talking about.
But it can also be used as a stand alone decision reasoner, and what
I\'m going to be talking about is a way right here that it is a
standalone architecture using utility. If you want more information on
it, again, that is what my book is primarily about. And then also here
at the AI Summit both in 2010 and 12. Kevin, Dill and I lectured on
extensively on utility systems. Both of those lectures are available on
the G. Not only with the vault pass that many of you will have, but also
up there for free courtesy of GDC, which is great. And then I\'ve
written about it occasionally at other places like Game Developer
magazine. The thing is, after all of that, I still have gotten people
coming to me with their questions and problems. With things like, how do
I standardize all of these inputs that I might use for my utility
system? Or define the the formulas and response curves that I\'d be
using. How can I make it so that I can easily extend this as my design
grows? With minimal code adaptation, but still make it very designer
configurable, because after all utility systems do have lots of knobs
and sliders. And numbers for designers to be able to tune and tweak
with. And so these are all very valid. And that\'s really what the the
infinite access system was meant to kind of address. As I was answering
these problems. Now of course, you may say the IAU SSI don\'t believe
they exist. But it\'s very a really simple architecture at its core. For
every action that our characters could take, we\'re going to have one or
more considerations or axes. You\'ll see why a little. Why I call them
them axis that is being used to weight the utility of that action? And
each of those axes is going to have an input and some parameters to work
with. We\'ll get to that in a. The important thing to note here is that
this is just a list of axes. Of considerations that are important for
that action. So if we come along later and say, you know what, we
actually need to take some more information into account, we can just.
Add axes to this list. Theoretically, infinite number of them. Which is
when the name kind of comes from in a tongue in cheek sort of way. So
each action could have a number of different axes that it uses to make
to weight that action and of itself, and they may overlap. They may use
some of the similar ones. May use very unique data specific to that
action. The point is because it\'s just a list structure we can push as
many of these. There as we want you. So what are these actions and axes?
Let\'s take some examples if I want to decide, do I need to use my
health kit? Obviously something I might take into account would be
backup. We. What\'s my health? You know, do I need to use the health
kit? Umm, if I were to decide if I was going to be, you know, attacking
somebody like like Dino or Steve or something. Thing. Well, how far are
they away from me? Are their. How much ammo do I have? Moving to cover,
if I was to. Well, what am I being attacked? I need to take cover. You
know how much health do I have? I need to. How far away is the cover
from me? It the podium. Is it? You know the back of the room. Whatever.
The quality of. Is it a brick wall or is it a stick? Are all
considerations that we. Taking about making those actions now you notice
that some of these are specifically on a per target basis. Was the the
distance to Dino compared to Steve compared? The ca. So that\'s. We\'re
just going to be doing those on a per target basis. So for example, you
know if I was deciding to shoot somebody there maybe 3 targets out
there, I\'m going to score each of those targets individ. And so in this
case, target a may be the highest score. Same thing with hiding. If
there\'s three cover spots that I could take, I\'m going to rate those
individually with their own scores, whereas things that don\'t
necessarily have a target like healing myself or reloading my gun, we
just score it as a whole for the action. Now, once we have these scores,
we push the scored actions. Onto just a list of possible things we can
do and we\'ll get to that in a moment. First thing I want to focus on is
this part right here. What are the score? How do we score these actions?
Well, we\'re going to be passing things through response curves and a
couple people have mentioned response curves already here at the summit.
But a response curve is just a mathematical representation of a
relationship of some sort. It\'s mapping a concrete value onto an some
sort of associated abstract value, like distance to threat or how much
health do I have compared to the my necessity to heal at the moment or
to reload or whatever the case may be? The cool thing about response
curves is they are easy to graph and to visualize. Can see that
relationship in your head. But what is really neat and something I\'ve
been focusing on is I found that I was defining response curves over and
over in the same way. First of all, a curve type. Is it? Is it a? Is it
a logistic or a logic curve? And then four parameter values. Mkb and C.
Not xkcd. That\'s different, but MKB and C. 1st the curve types and
I\'ve talked about this in those other lectures I see. I would recommend
you take a look at those, but a simple linear function or an exponential
one like a parabola. A logistic function or a logit function which is
almost the logistic on its side. So we choose our curve type and then we
go on to the parameters for a linear or quadratic one. It\'s all based
on this. You familiar with the slope intercept formula. I tweak it a
little bit. So that we can get all four of those parameters in there and
you\'ll see why in a moment. But M is just the slope still and K is an.
So how much of A of a bend do we want in our parabola be the Y intercept
is really just a vertical shift in. Correspondingly, C is a horizontal
shift. And so that\'s what those four parameters do for the linear and
the quadratic. The logistic curves the parameters. Your base logistic
formula. Again, I parameterize it a little bit more, so it\'s that
you\'ll notice that all four of the of the parameters are in there. M is
just the slope of the line at the inflection point. So right there in
the center K is actually the vertical size of the curve in this case. So
we can make it taller or shorter. And then B&C again are just moving it
up, down, left or right. So again, we have 4 parameters that we can use
to define this. So what does it look like then to define any of these
response curves with this format? It\'s this the type of curve, and then
the four. So if we had this and say a linear curve with a slope of 0.5.
And K is one because it\'s linear and we\'ve bumped it up a little bit
by .2. There it is. Just with those. Same thing. Here is a quadratic
curve. With those numbers it looks like that a logistic curve with those
numbers it looks like that. It\'s very very simple to define these
things how we want them to look. And we\'ll come back to that in a
moment. So all I\'ve done is I just created a real simple worker class
for processing response curves that takes a function call with what\'s
my X value and then the parameters for the curve, clamp the input to 0
to one. Come back to that in a moment. But that\'s important. Calculate
using the appropriate formula type depending on the curve, clamp the
output to 0 to one and return Y. So that\'s all that this class does is
process these things for us. But putting it into the hierarchy now is of
course we have our action as we as we talked about and it has a list of
possible considerations. Axes, right? Has an input and the parameters
and. Again because it\'s a list, we can decide how many of these axes do
we want. We just push them. Now here\'s the key of getting these axes to
play together is we\'re going to take that list and combine them
together by multiplying them. Now again, as I mentioned, the output of
all of these, these response curves is normalized zero to 1, so you
multiply them together. Output itself is already normalized for you from
zero to 1. There\'s our action score for this action is we just
multiplied whatever the response curve has told us about those these
axes. That\'s important because if we come back later and say, you know
what, we want to add a fourth axis to this because again, we\'re
multiplying. It doesn\'t matter what it is, we\'re going to multiply it
together and still have a normalized output for our action score. And
that\'s really important because we we can continue to add axis as we
need them and maintain that normalization and that comes in handy later.
Going to right now, focus on the the input portion of this. Know what
are these? Well, inputs as we know are going to be. What\'s X in the
equation for the response curve? So all it is is what sort of
information do we need to reason on for this particular? And The thing
is, these can come from a lot of different places. Personal info like my
health, my ammo, or information from other objects or agents in the
world. His health. Strength. What\'s the value of that power up over
there on the floor or world info like the distance between? Two objects
in the world. How many allies or enemies do I have? How much time has
elapsed since a particular? I mean, that\'s a world info. I think we can
use it as an input. Or any other aggregate info that we could think of,
like stuff we could get from a blackboard or from an influence map. So
all these are potential. Inputs now, as I mentioned earlier, normalizing
the inputs is very key here because the X range for our response curve
does run from zero to 1. And so we need to normalize these. Some of it
is normalized by default, like anything with a percentage, a health
percentage, AML percentage. Don\'t worry about that, right? But what
about more cumbersome data that isn\'t necessarily going to behave quite
as as conveniently for? What I do is I define bookends with 0 being the
least that I care about and one is the most that I care about and
anything outside of 0 to one. I just clamp appropriately. And what that
means is, for example, if I had a distance input as one of my axes, I\'m
going to pick a maximum distance beyond which everything else is. Just
going to treat as the. So in this case, maybe after 50 meters, you know,
for all intents and purposes, it\'s far away. So if I were to have,
that\'s my 1.0 right there. I were to have something at 70 meters. I\'m
just going to clamp that and say, you know what? It\'s still 1.0. We\'re
going to treat it the same as 50. And that actually works out a lot
better than you would. It\'s very convenient way of just saying at a
certain point, I don\'t care. Another example will be I might treat
differently if I had one enemy or four enemies or seven, but after eight
we\'re just going to say lots of enemies. And so if I had 11, I\'m just
gonna clamp that again to being X = 1 point. There\'s other things we
could do that would have variable maximums depending on the situation.
What weapon am I? We\'re going to use the range as the normalization for
the range of that weapon, or how fast can I go? My range might be how
far could I travel in three seconds? That depends on my speed. If I get
hit by a slow spell, that\'s going to change the normal. Realization is
gonna be elastic along with. We can even glue these things together and
this is what I\'ve actually used a number of times. Is my weapon range,
plus how far could I travel in three seconds for using attack decisions
for example? And that\'s just going to be elastic. To keep that
normalization in place. Well, these inputs of course come from
different. Some of them from my own objects, some from other objects,
some from the world data. And so in order to keep track of the mall, I
actually enumerate the possible function calls that we would want the
possible inputs. And let\'s face it, a lot of times we are calling the
same data. Matter what it is a distance function for example. A lot of
things would use that, but if we enumerate it then we can put create a
sort of clearing house that returns that normalized value based on
whatever that enum is. So we might have some place in our file something
looks like. All our inputs and. Significantly larger, but here\'s an
example health ammo. The three second range I talked about distance. And
there\'s going to be a corresponding switch statement someplace. Some of
them might be very simple like this. Another one might have that 3
second thing built in. I might have another input that says a 5 second
range, you know, and it would do the same thing and some of them are
going to actually build the normalization code into it, like the
distance from the with the clamping and everything. All we have to do is
say. Distance and it returns the normalized value with the maximum
distance we want to consider, et cetera. So here\'s the flow now, for
every action, we\'re going to take, we\'re going to loop through our
axes, and for each axis, of course, it has a definition with an input.
That we\'re going to pass to that that input clearinghouse, it says go
get me the appropriate piece of data and return it back to me. \'cause,
I need that for X. Now the input definition also has those parameters.
Has the curve type and the. And we\'re going to pass that to the
response curve calculation. To get back. Why? Finish looping through
our. Multiply them together as we talked about, it gives us our score.
Now this the the object is now the action, the target if I have one.
Know like shooting Steve instead of Dino. Sorry guys. And then the score
for that action, we push it onto that potential action list. And so all
we\'re doing at that point then is we can calculate, you know, these
things push it onto the list and then select from that vector using, you
know, the highest scoring 1A weighted random. However, we want to do it.
The bottom line is we have. Our scored outputs right there. The thing
about this system is it really lends itself well to designer tools
because after all, all of the information is in that data that we\'re
using to construct the axes and the actions themselves. So you can make
standalone designer tools like a winforms app for something to. And
manipulate. And even export the data when you\'re done with it right
there from the. I\'ve actually done two different ones of these for
clients using a variance of this system. Unfortunately, such as the game
industry, I can\'t show them to you. However, it doesn\'t really matter
because they\'re all design dependent anyway, as most tools are. The
important part of this. Is you have to be able to give your designers a
way to visualize this information. And so I\'ve actually created an
Excel based visualization tool that I just open up occasionally so I can
plug stuff into it and model it. And that\'s kind of the basis of what
I\'ve done in the actual designer tools. But for example, if we look at
here, there\'s an Excel spreadsheet and down there I just have different
tabs for the different types of formulas. And here\'s my MKB and C here,
and I even put some presets just as reminders for different starting
points that I work with. Now I recorded this and this is just me
tweaking parameters in this Excel. Sheet. Here we go and notice how we
can move the curves around up and down. Change the slope, change the
angle. The exponential 1. So notice that we can get all sorts of
different shapes just by changing these numbers and moving them around
to get them. To to the point where they are reflecting the type of
relationship we want in that data from an input to an output. And by the
way, at the end there will be a link that you can actually just grab the
spreadsheet and play with these numbers yourself. So the thing about
this is it\'s all based on these mathematical considerations for the
action scores themselves. 1.0 means that the situation is absolutely
perfect to take this action. \'Cause it\'s the maximum number we can
have, whereas on the other hand, 0 means that the action should never be
taken in this situation and most of the scores are going to fall between
that and some sort of range 0 to one, we\'re going to use that to. Them
to the other possible. Actions, right? Well, the axis remember that
we\'re multiplying together. That\'s important because. A0 In any one of
those those axes, any one of those inputs means that the action will
never be taken because we\'re multiplying them. Zero is just going to
negate the entire action right? On the other hand, a 1.0 for the action
is only possible if all of the axes are at 1.0, meaning. The stars are
in alignment. The distance is. The health is right that all of those
things, this is the perfect situation. So from each individual axis it
bubbles up to the hole. So running through some examples of of how this
would look and it becomes clear when you see it in action. Going back to
that, selecting a target thing, well, let\'s say we want to consider the
distance to the enemy as something we want to consider for a melee
attack, for example. There\'s my my maximum distance in in. How far can
I go in three seconds? We use that as my my bookend. That normalized
input and using that. Formula there in yellow. It gives us this curve.
Does that mean? Well, if somebody\'s close to us, well, he\'s going to
be pretty high priority. He\'s right. I might as well attack him. But if
he\'s far away, I might have it fall off so that I\'m not as interested,
even almost, to the point of 0. Notice I said almost. Notice that
doesn\'t go down to 0 there. The reason for that is what if somebody is
4 seconds away? Or five. Now remember, we\'re just going to be clamping
that. Input to that myspeed times 3. We still want to consider stuff
that\'s out beyond that, and this still will if he\'s 4 seconds away.
I\'m just going to treat him the same as 3 seconds or five or ten will
be treated the same as 3 seconds, so that.

Yeah.

An important thing to note there. But we also want to consider health
\'cause you know what\'s probably not a good idea to attack somebody if
I\'m really wounded and so using that formula gives us this curve. I\'m
healthy. That\'s fine. 1.0 just use the distance alone as your
consideration. However, once you get low on health, start pulling that
down a bit. This notice it does. We go down to about 50%. I\'m going to
be multiplying that by the distance. So whatever the distance comes back
with is going to be halved. If I am almost dead because we\'re
multiplying them together. These two are playing together. There\'s a
50% healing my ally. Another example. What\'s my allies? You know, if
he\'s hurting, I might. It might be important to heal him. On the other
hand, if he\'s not, why bother? How far is he from me if I don\'t want
to have to run farther to to heal somebody? If there\'s somebody close
like like Luke, he needs to heal. So but notice it also doesn\'t go down
to 0 again, just like we talked about. Now, how about if your designer
comes to you and says, you know what, we don\'t want the guy using the
last of his mana to heal somebody else because we want other things to
bubble up, like a self preservation. Spell or running away or something.
We need to leave room for. So putting this curve in here again with the
yellow formula on the left. If he\'s got a lot of mana, that\'s fine,
but as he starts to run out, we\'re going to push this down, even to the
point of being 0. So even if somebody if Luke is right next to me and
he\'s really hurting, if I\'m down to 10% of my mana. I\'m not going to
heal him because I need to do other things instead, and that\'s
automatically going to happen because we\'re multiplying these together.
But what if your designer then says he\'s still using too much of his
low end mana? Change. I want to push that over a little bit. Maybe to
20% is where it falls. Well, by changing this number right here, which
is the slope, by the way, from -1.5 to -2.5, it changed the shape of
that curve over there. It is pushed. So by changing 1 number. We\'ve now
changed how Mana is included in the overall idea for healing. Another
example, moving to. I\'m not going to step through this as quick other
than am I being attacked? What\'s my health at the time? I need to. How
far away is that cover from me? Close cover is preferable to cover. Far
away. What\'s the quality of the cover? If the quality is low, I don\'t
want to consider it as much. If it\'s high, that\'s fine. There\'s five
axes right there, and we can actually come back and put in another axis
we want, and it doesn\'t matter. Going to be. Dealing with one thing at
a time that then gets combined into the hole with all of the other axes.
Other tricks you could use using. For example, how about building your
cooldown timer right into an axis? How long has it been since I\'ve last
done this action? If it\'s been recent, don\'t do it again. But as that
timer. Goes OK fine after a certain period of time, I\'m going to start
to allow you to do that action again just by adding another axis. The
reverse of that would be a repeat limit is fine if you want to do
something like two or three times in a row, that\'s great, but after you
repeat it a few times, I\'m going to start cutting you off. You can also
do other things like triggers like Boolean. I don\'t care if there are
10 guys lined up like bowling pins on the other side of the room. If you
are inside, there is an axis that passes a 0. To this you multiply it by
zero. It doesn\'t matter that they\'re right. Don\'t throw a grenade at
them because you\'re inside A room. Cool. This is something I borrowed
from Kevin Dill last year. He did it in a slightly different way. But
what I do is I add a weight coefficient to the action as a whole, so not
to the axes, but to the action. And we\'re going to multiply whatever we
score that action as with the axis by that weight, and it allows for
action scores of greater than one. One and it kind of allows us to
define an implicit priority structure, much like the the hand authored
behavior tree priorities by putting them in a certain order. But this is
dynamically flexible. So for example we might say idle actions, we\'re
just going to leave at times 1. Combat actions might be times 3 scripted
actions might be multiplied by 5. An urgent reactions like a. Dodging a
grenade for. We\'re going to multiply by 10. Well, what does that look
like here? If we had three axes on this action? Here\'s their. They come
back, we multiply them together. There\'s the final score for the
action. This is a combat action. The action has a weight of 3, so we\'re
going to multiply it together and it gives us 1.4 approximately. So
notice that by multiplying it by the action weight, we can actually have
something that\'s higher than 1.0. There is no idle action that is going
to supersede this particular combat action at the moment because the
idle actions are just going to be 0 to one. So that gives us a situation
like this. Even a scripted action that\'s multiplied by 5 right here.
That\'s the maximum we\'re ever going to see. Even if the Dodge grenade
action comes back as a 0.6, it\'s still going to supersede. Taking a
scripted action because we\'re multiplying it by this weight of 10. So
in summary, here\'s the whole. Actions can have one to any number of
actions. This clearinghouse returns us our normalized input value.
We\'re going to pass it through the response curve with the parameters
that are part of the axis that give us a normalized output for that
axis. Multiply those. All the axis values so that we get an action
score. And one thing I left out is multiply it by the weight. If we
decided to do that. But then push that action onto the potential action
list so that we can select the best one. And it\'s all because of the
way those actions are designed in a list to play together with the
appropriate math. The advantage is of course, that they\'re really easy
to define the action considerations we saw just add an axis with an
input and some parameters, and you\'re done. It\'s. We can come back and
add. Actions and axes very very fast. And the processing is pretty quick
too, because hey, it\'s just math and rumor has. Computers do that
fairly well, and it lends itself to some very simple tools. Even an
Excel spreadsheet or some sort of designer tool to be able to to roll
through these quick. And it\'s also then easy for designers to
visualize, tune and tweak. Because they can see it on the screen, they
know the relationships. Anyway, this is my contact information and at
the bottom here, if you\'d like to play with a copy of that response
curve spreadsheet. Just to see how these numbers play together, you can
do it at that URL and I can\'t believe that we have a minute and 45
seconds for questions. Thank you. Graham.

Hey I have a question for Dino.

Have you looked into functional reactive programming or monads as a
solution to this?

Closest thing I\'ve seen to this this kind of functional reactive
programming is the closest thing I\'ve seen to it, so I\'ve just only
learned about that recently, so I\'ve been looking. That yes.

For Dave, do you find that you have to tune all the axes at the same
time, or can you tune them independently? If you had like a 10 axis
action versus 2X action, even if they\'re all .9, they\'re going to
multiply the less. The two axis one. So do you have to tune the mall
together at the same time or?

Yes and no. You know, because of the fact that the response curves are
about the one axis and its relationship with itself of that input to how
do I want it? You\'re thinking a lot of times in terms of that one, but
you do have to keep in mind that it\'s not going to be the only one.
Playing that game with everybody else and so you know, it\'s like if you
say, well, I\'m going to scale this all the way down to .1 at the least
desirable. Well, that\'s a .1 that you\'re going to be multiplying
against everything else. What if everything is at .1? You know you\'re
going to get a very, very low number. And so yeah, some of the the axes
that I use, I think I had one of them in there which a very shallow
curve. You know, just as a line. Just so for example. All other things
being equal, and you say that a lot working with the system. I want to
take something that\'s closer to me than farther away, but that again,
it\'s because of that, all other things being equal. Now, once the
system is running, they aren\'t. But it\'s almost an emergent sort of
thing where these things are working together to create. A larger number
for that that action, and then of course the actions compared to each
other. So yeah, you have to run that balance between what am I looking
at for this action compared to, oh, yeah, by the way. It\'s also it
needs to be a fox rather than a hedgehog. For those of you who are here
yesterday. So yeah, it\'s, but it is more of an art than a science at
that point of keeping track of that. Now, I\'d love to use up Paul\'s
dynamic tuning stuff on that. Paul and I got to go have a beer and talk
about this, but so anyway. Go ahead.

This is for you, Dave. How do you? Debugging the utility based
behavioral mathematics axis stuff. You go in and you see an AI do
something that doesn\'t make any sense to you. How could it possibly
come back? And you go to investigate. What sort of tools do you build
into your systems that make debugging that sort of problem easier?

One of the key things to do is make sure you have you know on screen
stuff of okay. Here\'s, you know, select A character. Here\'s a D list
of the list of things that it was considering. Then, if possible, the
axes that we\'re going into that, and you can say, well, maybe it\'s
because. Oh, you know what? It seems to be only because here. Why is
this number point? Do you? And that\'s what\'s cutting down everything
else. Let\'s go back and change that one axis. And by the way, those the
the parameterized things I was talking about with the type and the four
parameters you will get to the point where you can just look at that
parameters and go. Let\'s just change this to up to .3 instead and you
will visualize it in your head. It gets to be a vocabulary at that
point. But it. It is a little more difficult than a very rigid scripted
or or a deterministic way. Of this was there. This was there, this was.
Therefore, he should be doing this because this stuff does ebb and flow,
and it is fun to watch your debug stuff as an agent moves through the
world, just you know, up and down his various scores change. But that\'s
the important thing is the debug stuff on screen. Then being able to
think in terms of that system so.

As a follow up. As a follow up, have you considered using that sort of
communicating? That sort of information to the players so they can
understand the reasoning of the?

Bots. Yeah, certainly. That\'s a design decision. Depends on what it.
You can certainly use the axis and the action scoring information to
drive things like the barks that Jeff Orkin was talking about, or
whatever the case may be. Absolutely.

Anything.

So we are done. You can find me. So thanks folks. Appreciate it.

![A black and yellow text on a black background Description
automatically
generated](../../images/media2/image1.png){width="5.850826771653543in"
height="2.7565212160979877in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image2.png){width="4.878261154855643in"
height="3.2792596237970253in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image3.png){width="6.1785968941382325in"
height="2.580042650918635in"}

![A diagram of action and action Description automatically
generated](../../images/media2/image4.png){width="6.439130577427822in"
height="2.9044586614173227in"}

![A black background with yellow text Description automatically
generated](../../images/media2/image5.png){width="6.3138221784776904in"
height="3.2812193788276467in"}

![A graph on a black background Description automatically
generated](../../images/media2/image6.png){width="7.795651793525809in"
height="3.8646194225721784in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image7.png){width="7.280860673665792in"
height="3.4350240594925636in"}

![A graph and diagram on a black background Description automatically
generated](../../images/media2/image8.png){width="7.284763779527559in"
height="3.4921806649168854in"}

![A screen shot of a graph Description automatically
generated](../../images/media2/image9.png){width="6.643478783902013in"
height="3.4505325896762904in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image10.png){width="8.055581802274716in"
height="4.674426946631671in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image11.png){width="9.47617125984252in"
height="4.801359361329834in"}

![A black background with white text Description automatically
generated](../../images/media2/image12.png){width="9.253331146106737in"
height="4.508424103237095in"}

![A graph on a black background Description automatically
generated](../../images/media2/image13.png){width="7.195651793525809in"
height="3.2912182852143483in"}

![A graph on a black background Description automatically
generated](../../images/media2/image14.png){width="7.9321817585301835in"
height="3.7391907261592303in"}

![A graph with blue arrows and yellow text Description automatically
generated](../../images/media2/image15.png){width="6.971422790901137in"
height="3.6014195100612425in"}

![A screenshot of a computer game Description automatically
generated](../../images/media2/image16.png){width="5.934782370953631in"
height="3.4753226159230097in"}

![A black and yellow text Description automatically
generated](../../images/media2/image17.png){width="7.582385170603675in"
height="4.468720472440945in"}

![A screenshot of a computer program Description automatically
generated](../../images/media2/image18.png){width="7.56956583552056in"
height="3.7812018810148733in"}

![A diagram of a flow chart Description automatically
generated](../../images/media2/image19.png){width="7.5674726596675415in"
height="4.233575021872266in"}

![A black background with white text Description automatically
generated](../../images/media2/image20.png){width="7.596097987751531in"
height="3.844897200349956in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image21.png){width="8.49963801399825in"
height="4.284653324584427in"}

![A diagram of a structure Description automatically generated with
medium confidence](../../images/media2/image22.png){width="7.991610892388452in"
height="3.866908355205599in"}

