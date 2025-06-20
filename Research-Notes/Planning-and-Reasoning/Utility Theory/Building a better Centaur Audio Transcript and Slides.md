Audio file

(Audio) GDC2015-Mark.m4a

Transcript

Welcome to building a better Centaur AI at massive scale. Now it\'s
supposed to work.

OK.

As many of you know, my name is Dave Markham, the president, lead
designer of Intrins. Algorithm in Omaha, NE. I\'m also, as you know, the
Co host of the AI Summit and Co, founder of the AI Game Programmers
Guild, with Steve Raben. The author of the book Behavioral Mathematics
for Game AI came out about seven years ago. I\'ve written for a lot of
other books. On a variety of topics and I was also an occasional writer
for the now defunct Game Developer magazine. But I do AI consulting now.
This is part of what? Why I\'m here right now for a variety of companies
over the years, most recently working at Arena net. On Guild Wars 2 and
the expansion heart of Thorn.

I\'m Mike. I\'m a lead game play programmer at Arena Net, also working
on Guild Wars 2 at the moment and I was formerly working on Space
simulation series from a German. Called egosoft. I\'ve been a forum
moderator on gamedev.net for a number of years, and I\'ve also written
for the game AI Pro series as well.

The reason we\'re here is Mike and I have been working side by side on a
project for about 11 months now. We did not coordinate our shirts that
day. And that brings us to this lecture. Thought we\'d share this with
you. Yes, this project was for an MO, but the architecture is not MMO
specific. The concepts you\'re going to see here would work just as well
for a shooter, an RPG, or even a strategy game for that matter. The
components themselves are relatively simple to implement. As always, the
challenge with something like this is going to be hooking it up into
your engine and your world. We\'re going to be telling you about what\'s
going on under the hood.

![A white paper with black text Description automatically
generated](../../images/media2/image1.tmp){width="4.270865048118985in"
height="3.072939632545932in"}

So, as with all good things, this comes with a bit of a disclaimer. The
talk we\'re giving here is entirely about architecture. It does power
the expansion for Guild Wars 2, Heart of Thorns. However, we will not be
showing any exclusive heart of thorns content today.

Some of the behaviours you\'re going to see out of the system in our
demos. Are in heart of thorns, but not everything you see will be in
heart of thorns. Go running out of here. Going. Oh, my God. Oh my. Heart
of Thorns is going to do this. That may not necessarily be the case.

![A close-up of a white page Description automatically
generated](../../images/media2/image2.tmp){width="4.729201662292214in"
height="2.838561898512686in"}

What we\'d really like to focus on is what the architecture itself is
capable. Love and to give you a glimpse of some of the things that we
might be able to do in the future at our Internet with this system. When
we started the project out, we had a sort of mandate of a number of
goals that we wanted to hit and the the first one that we\'ll talk about
here is the AI execution, which is just the behavior of agents in the
game. You\'re playing. Wanted to make sure that the characters were able
to use their skills and their spells and things in a much more
intelligent and reasonable way. We also wanted to promote better
movement, tactical maneuvering and that. And all of that is empowered by
having better environmental awareness within the engine itself. And that
leads us to the ability to create a wider variety of behavior archetypes
so we can have a lot broader. Depth to all of the creatures that we have
in the game. We also wanted to make sure that some of the old infamous
exploits in the AI were taken care of and we had to do all of this.
Interestingly enough, with a even stricter performance and memory budget
than we originally had.

![A close-up of a list of skills Description automatically
generated](../../images/media2/image3.tmp){width="5.796917104111986in"
height="2.973979658792651in"}

The other thing that we wanted to. On was improving the designer quality
of life, so to speak. We wanted to be able to support more actions that
the designers could use in creating creatures to give them that broader
range of of of. Behaviors. But we also wanted to be able to have more
autonomous behaviors from the creatures so that they weren\'t so tightly
scripted. And of course, a lot of times when you have things that are
scripted, you have brittleness that results. We wanted to be able to
save the. From those brittle behaviors. But most importantly, we wanted
to be able to encourage faster development time of these creatures,
because in an MMO, especially when you have literally hundreds of
different types of characters, being able to iterate on them quickly.
And create the AI for them is paramount important.

So.

One of the key pillars we had for maintaining that iteration speed was
building a data-driven architecture and we already have a heavily
data-driven system in Arena net which is powered by our design tool
which we call. Now we try to maintain a distinct. Between code and
content, and we\'ll be illustrating how that works as we go along today.
At the high level, though, the Content division from code is basically
like this. Content is external XML data. All designer edited. It
provides configuration and parameterization to the code itself, and it
allows recombination of prefabricated code elements into new and unique.
Behaviors. Unfortunately, all of this is. It does not change at all.
While the game itself is running. Code is of course code and it
implements the building blocks that content relies. But more
importantly, it consumes content to know not only what to do, but how to
do it. This gives us a lot of dynamic flexibility and power and allows
us to recombine elements at runtime as opposed to just at design time.
Oh yes, it\'s also sorry, this also gives us the capability of
maintaining state information about what\'s going on in the world as
agents perform their actions.

![A white text with red text Description automatically generated with
medium confidence](../../images/media2/image4.tmp){width="4.4271161417322835in"
height="3.22919072615923in"}

So getting onto the architecture itself, there\'s actually four
different components of the architecture that we\'re going to be talking
about today. First of all, is the infinite access utility system which
handles all of the decision making of the characters. There\'s also a
modular influence map. Architecture underlying this that allows us to
provide world information. And there\'s a robust content tagging system
that we. So we can provide characters with contextual information, not
only about each other, but about the world in general. And then also we
did a little bit of work with some more IK head look. Did you give the
characters this feel that they had a better presence in the world That
they were aware of. And so we\'ll go through each of these in turn here.

![A diagram of a diagram Description automatically generated with medium
confidence](../../images/media2/image5.tmp){width="6.0469192913385825in"
height="1.8229297900262467in"}

First of all, the infinite access utility system is the core decision
making engine that we\'re going to be talking about. Some of you may be
familiar with the I, I, USI talked about it here. Two years ago at the
A. And the lecture is of course available on the GDC vault, and I very
much recommend going back and looking through that. Going to tell you
how and why the iaus works underneath the things we\'re going to be
talking about today. Interestingly, I found out that five weeks after
that lecture, Mike began implementing. The Iaus and the reason I found
that out is when I got on site and I started looking at his header
files, I realized he had actually been dating them. Five weeks after
GDC. So he had gotten kind of put his toes into it. But when I got on
site last year, we really started kicking off the development of this in
earnest. So the iaus has these atomic actions that are just individual
behaviors at its core, but each of those actions has one or many
considerations or axes. Mathematical models, which is why we call them
axis. You can have as many of them as you need to describe why you would
do this action on each consideration has an input and some parameters
around it. Parameters include things like a curve. Type of the shape of
the curve and then further being able to shape it by the parameter
values we put in there. So that gives us a lot of possibilities of
different response curves that we can run these inputs through and
we\'ll cover these a little bit further. Most importantly, though, for
this system is we actually normalize all of our inputs because on these
response curves the X range runs from zero to 1. We need to normalize
the inputs to match. So some data that we\'re used to using is
normalized by default, things like. Health percentages or how fast am I
going relative to my maximum speed? There\'s a lot of cumbersome data
that isn\'t normalized by default. So what we do is we define what we
call bookends, and so 0 is going to be the least that we care. One is
the most that we care about and we\'re going to clamp anything outside
of that to zero and one respectively, and so we can parameterize these
bookends to mean whatever we want. Could be a range of distance that
we\'re concerned with. Or the number of enemies in a certain range that
we\'re going to make a decision about. So the hierarchy is this. We have
these actions and as I mentioned, they can have a whole bunch of
different considerations. We have these in a list and each axis has its
input and its parameters as I mentioned. So we have as many of these as
we want as we deem necessary. And what we\'re going to do is we\'re
going to combine them the outputs of them by multiplying them together
to get a score for that action. And this is the things that will be in
the the lecture from two years ago that\'ll go more in detail about how
that works. But The thing is, for each of these actions that were could
potentially perform, we\'re going to score them if an action has, it
could have a target. We actually score it on a per target basis. I
attack him at him, him different scores. And so we go through an all
possible actions with all possible targets are going to get a score, and
then when we\'re done with doing that, we push that onto a list of
potential actions that we could select from and simply pick the highest
whatever scored the best. One thing I did not mention two years ago and
I want to put it in here for completeness, for those of you might
implement this is what we call a compens. Actor. Because if you think
about it, as you multiply these normalized values, the total is going
to. So if we start with .9 and multiply it by .9 a few more times as you
see, that number is going to drop through the floor. Eventually we just
with five of them that we\'ve gone from .9 to point 5.59. So what
happens there is even having a lot of high scoring. Reduces is the score
dramatically and so kind of punishes you for having more considerations
which really doesn\'t fit well with this whole infinite axis thing. So I
want a quick shout out to my friend Ben Sizer, who I worked with this on
a prior project, pointed this out to me and talked me through a couple
ways of of. That I\'m going to go through it in detail, but what we did
is we added this compensation factor to self balance it somewhat. It
takes into account how many considerations do we have and adjusts the
score accordingly. So looking at this situation, if we have 6
considerations of .9. Instead of .9, they\'re going to be treated as in
this case .97. The bottom line is this. The red bars is without the
compensation factor and you can see it dropping. The blue bars is with
and so it does make it more elastic and stays true to what you\'re doing
with the scores. So anyway, all of this goes into this big architecture
that we\'re going to be talking about and what we\'re going to do is
we\'re going to start from the inside and work out. So we\'re going to
be talking about inputs and parameters. Right now, inputs can come from
character data objects like my data object or a target. They might be
considering. They can come from the game engine like what is the
distance between objects or how much time is elapsed since a particular
thing occurred. We also can get inputs from our influence map system,
which will address later. The thing is, each of these is predefined so
much so that we can actually put them into a drop down list in our
design tool. So in this drop down list you see all of these right here
are inputs. And we actually, this is actually a scrollbar over here. So
we\'re only looking at about a third of our predefined inputs. Are
information that we can pluck. Out to work with. So examples, if we were
doing some sort of melee attack. We would be interested in things like,
well, how much health do I have? How much health does my target have?
What\'s my distance to the? Certainly. And do I have a valid line of
sight to him if he\'s? Or is he on the other side of a wall where I
can\'t even see him? So all of those are inputs that we might be
concerned with.

![A screenshot of a computer Description automatically
generated](../../images/media2/image6.tmp){width="5.9844192913385825in"
height="3.2552318460192478in"}

![A graph on a white background Description automatically
generated](../../images/media2/image7.tmp){width="6.734424759405075in"
height="2.9843963254593175in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image8.tmp){width="6.760465879265092in"
height="2.807311898512686in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image9.tmp){width="6.729215879265092in"
height="3.395858486439195in"}

![A white text with black text Description automatically
generated](../../images/media2/image10.tmp){width="5.588582677165355in"
height="3.4062751531058617in"}

![A white paper with black text Description automatically
generated](../../images/media2/image11.tmp){width="5.260454943132109in"
height="3.22919072615923in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image12.tmp){width="5.802125984251968in"
height="3.697944006999125in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image13.tmp){width="6.364630358705162in"
height="3.3906496062992124in"}

So the code for this is actually really straightforward, but we want to
make sure and define our terms as best as we can. So when we talk about
a context, what we\'re referring to here is a package of information,
and that includes a number of things ranging from. Decision identifier,
which is just an enumeration that says what am I going to try to do? We
have a link to an intelligence controller which is basically saying
who\'s asking for this input information. We have a link to content data
which has parameters which are those bookends that Dave talked about
earlier and that specifies what do you need to know. And finally, we
have an optional link to a target object, which could be another
character. Could be a. It could be a well, it could be anything in the
game and we\'re basically asking the question who am I going to do this
action to? So an example of the code is we\'ll look at myhealth
consideration here. Quick. Basically all we have to do is pass in this
decision context package and from it we\'re going to retrieve the
intelligence controller. Ask the intelligence controller who are you
your character? And then take that character\'s current health, divide
it by its maximum health, and there\'s our normalized input. Target
health is actually very. We\'re going to do the same thing with the
decision context coming in as a parameter, but instead of asking for the
agent that\'s looking for the information, we\'re going to ask, what
target do you have? And given that target, we can look that character up
again in the. Engine and once again. Just compute his current health
divided by his maximum health, and we have our normalized result.

![A white paper with text on it Description automatically
generated](../../images/media2/image14.tmp){width="4.989619422572178in"
height="2.932312992125984in"}

![A screenshot of a computer program Description automatically
generated](../../images/media2/image15.tmp){width="6.000043744531934in"
height="1.343759842519685in"}

![A screenshot of a computer program Description automatically
generated](../../images/media2/image16.tmp){width="6.0417104111986in"
height="2.713561898512686in"}

![A screenshot of a computer program Description automatically
generated](../../images/media2/image17.tmp){width="6.593798118985127in"
height="3.140647419072616in"}

So that\'s where the input information comes. Now inputs as we mentioned
earlier, get used in considerations or the axes in the engine and a
consideration or an axis is maps these inputs into the decisions
themselves. Can have as many as we need. As we talked about, they\'re
all normalized 0 to one. So theoretically, as I mentioned, we can have
an infinite number of them. So if we were looking into perhaps doing a
ranged attack of some sort, all of these here are considerations. That
we\'re saying, what is the information we might want to think about to
decide whether or not to do that ranged attack. Now they have different.
They have a name or description and this is just a colloquial name of
that gives us an idea. What is this? Is this input playing in this
decision and we\'ll look at that in? Moment. The input itself of. What
info are we processing and the response curve parameters to to define
that response curve? Say, what are we doing with this information? And
then perhaps? Parameters like minimums, maximums, tags, et cetera. So
looking at this in detail, this is a zoom up of one consideration on a
ranged attack we might say well, not don\'t do it when you\'re too
close. So what? This is what we\'re measuring the distance to the
target. That\'s our input from that drop down list. And here\'s the
response curve. It\'s a polynomial, and there\'s some mysterious numbers
over there. And then our parameters, we\'re going to be measuring those
bookends from zero to 1000. This case, that\'s the range we\'re going to
be concerned with. Well, the parameterization can have things like a
cooldown as a maximum like 6 seconds, or can have a range like zero to
500 and all those as soon as you select an input, it knows what
parameters might be important for that input, and we can just go. And
fill that out right there. We can have something. A flag of some sort or
a status. Only do this when I have the buff status and visibility and
again because we said my buff status, we can fill out the appropriate
parameters right there. Other things we can actually do, references to
other actions, so not for. After I did this other thing or only within
the 1st 5 seconds after I did something else. We can reference other
decisions.

![A screenshot of a computer Description automatically
generated](../../images/media2/image18.tmp){width="5.9167104111986in"
height="3.4948173665791775in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image19.tmp){width="6.3750470253718285in"
height="3.1771062992125985in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image20.tmp){width="6.432339238845144in"
height="2.744811898512686in"}

So an example of how this looks in code is we\'re going to look at the
distance to target consideration input and we start once again with our
decision context and you should start noticing a theme here. From it we
get the target agent that we\'re going to consid. And look at basically
where is he? Am I? How far apart are we? And then we\'re going to do
something interesting. We\'re going to pull in this content object
called a consideration, which has the parameters for minimum and maximum
range that we need to worry about. That lets us do this little bit of
math at the bottom, which renormalizes into those. And then we can clamp
that result and we have our final input.

![A screenshot of a computer program Description automatically
generated](../../images/media2/image21.tmp){width="6.604214785651793in"
height="3.151065179352581in"}

So I talked earlier about these parameters up here for the response
curve of what we\'re going to use to to process the data from the input.
And sure, yeah, it looks like a polynomial curve, but I have no idea.
What\'s going on in those numbers now? If you worked with the system
long enough, you can kind of visualize it, but it is still kind of.
Ache. So what we did is we map this all into a visual editing system. So
here\'s the components of a consideration that I mentioned earlier, but
on this screen, which we can just pop up right from the decision itself,
you see we have a list of all of the considerations and one of them is
selected at the moment and well, that\'s the. To target. There\'s the
input, there\'s the param. The. We\'re concerned with, but this box
right here is those parameters for defining the response curve, and by
tweaking those, of course, we\'re going to be changing what\'s
appearing. Here. So this gives us a really good overview of how are we
processing the my distance to target with regards to doing this ranged
attack. So over the course of that range, in this case zero to 1000, we
can see oh as the distance drops to 0. We\'re going to be less
enthusiastic about doing this ranged melee attack. Can actually. See
that visually and get an idea of how that response curve is affecting
the input. So if you were to step through all of the considerations for
this particular thing, you could really get a good idea of what\'s going
on for each of these inputs and how they\'re affecting the decision as a
whole. And of course you can tune or tweak them as you\'re heart
desires, but it\'s really a good way to say what\'s going on here. Now
of course you can build these things from scratch as we talk about by
tuning all the little numbers, but we found that we were often going to
the same places for things. We created a preset tab on here. Which has a
whole bunch of little. That do things like creating presets as starters
to work. So we have normal linear and inverse linear normal, logistic
inverse logistic 4 different types of of polynomials with A2 exponent
and we actually found ourselves using ones with the six exponent a lot.
Just a variation of these two, we call them six Poly. And so those are
things that we could start with for more esoteric stuff. We even have a
tunable sine wave and a normal distribution. So you could start with one
of these and then start tweaking it to get exactly what the type of
behavior you wanted out of a particular decision.

![A yellow square with question marks on it Description automatically
generated](../../images/media2/image22.tmp){width="6.34379593175853in"
height="2.2500164041994752in"}

![A screen shot of a graph Description automatically
generated](../../images/media2/image23.tmp){width="6.234420384951881in"
height="3.3437740594925636in"}

![A graph of a curve Description automatically generated with medium
confidence](../../images/media2/image24.tmp){width="6.364630358705162in"
height="3.5833595800524933in"}

So that\'s inputs and how they map into considerations. We\'re going to
skip over a bit and go from input straight to output and we\'re going to
be talking about decision. Now decisions are actions that are linked to
a code function. Could execute a. They could perform movement to a
particular. They could call an animation whatever they could even run a
script action that the designers were created in the game. Some examples
are we could have like Warrior, Axe melee. This is a particular skill
that a warrior could. He\'s going to beat somebody over the head with an
axe or move to safe spot, or play emote wave. Those are the types of
actions. That are linked to decisions.

![A screenshot of a computer Description automatically
generated](../../images/media2/image25.tmp){width="4.0885717410323705in"
height="2.7708541119860017in"}

So from a code design standpoint, this is actually pretty interesting
because all of the action. Code is game specific. There is nothing in
the architecture, whether it be calling into skills, running animations,
moving to a destination. None of that is actually directly relevant to
the AI architecture itself. So we can actually ignore the man behind the
curtain, so to speak, and just say go do something to our agents and
this is empowered by the code and content division that we. About
earlier.

![A white paper with black text Description automatically
generated](../../images/media2/image26.tmp){width="4.411490594925635in"
height="2.432309711286089in"}

So some decisions can have parameters as well. Some of them actually
require parameters, so if I, say perform an emote well, I\'d like to
know which one you say will wave or bow or whatever, or run a script
when we need to identify which script we\'re talking about. And so these
are easy enough in our design tool you can say well, the decision play
emote to individual well the the parameter that we need to put pass in
it\'ll just say wave. Other options that we could do is like allow
walking because there\'s a lot of running in Mmos. Don\'t know if you
noticed that. There\'s content tags that we can. So for example saying
move to scenario socket. Yes you can walk there and you\'re going to be
moving the scenario socket ambient poi. So that\'s the only ones that
this decision is going to consider.

![A screenshot of a computer Description automatically
generated](../../images/media2/image27.tmp){width="6.229212598425197in"
height="3.067730752405949in"}

So as an example of one of these parameterized decisions on the code
side, we\'ll look at the shatter decision, which is basically just
saying a line of dialogue. We\'ll pass in to this function a content
object which we\'re going to look into its parameter and pass that
parameter into the content searching system, and then from there, all we
have to do is tell the game engine have this character say this chatter
line based on what? Gave us.

![A screenshot of a computer program Description automatically
generated](../../images/media2/image28.tmp){width="6.531298118985127in"
height="3.2031485126859143in"}

So that\'s the output of the system. It\'s fairly simple. Now the inputs
and the outputs are wrapped up into two very similar sort of things. We
call them decision score evaluators or DSE. DS. They represent a
decision process that evaluates the inputs with the considerations it
scores it, and then if it\'s selected, results in a decision. Basically
Dscs are saying why am I doing what? There\'s 2. There\'s non skill and
skilled DSE and they differ slightly. Non skilled dse are directly
mapped to a single decision, whereas skilled dse\'s they\'re not
inherently associated with one skill. Need to be assigned later and
we\'ll look at that. A little further. But they do have common
components, of course. They have a name and a description. They have
their list of considerations that they\'re going to to use, and then
they have a weight and then some optional parameters. So looking at a
screen here, we\'ve seen the screen a few. But we\'re going to look at
it in its entirety now. This is a screen for a DSC. And of course it has
the name we\'re going to go back to our melee skill at the moment, and
it has a description. Now this is the wait and we\'ll touch on that in a
moment. And of course, here\'s the list of considerations that are going
into making the decision. Should I melee a person or? We also have other
optional play things like skill placement. And use on. This is like a
healing. Do we even want to consider our friends? Friends, so the
weights that I talked about, they\'re an implicit but not explicit
priority system. What I mean by that is they\'re not guaranteed to
stratify, as this always beats that we they\'re basically just general
guidelines that we put in to kind of give some hints as to what\'s more
important. So we do things like idols and emotes are one tactical move.
To skill usage as a weight of 3 and emergency things like getting out of
a of a bad area of affect spell, etc. It might be 4 or. That\'s really,
really important to you to do. So what we do is we multiply the score of
the considerations by that weight to get the final score for the. So
after we look through our considerations, we score each one of them and
we multiply them as we talked about, we get that score, but then we\'re
going to retrieve the weight for the DSC as a whole. It by that to get
our final score. So as you can see even. Outputs are normalized from
zero to 1 by multiplying the weight later. We definitely are giving
boosts to things like the skill usage like the the emergencies,
etcetera.

![](../../images/media2/image29.tmp){width="5.974001531058618in"
height="3.4948173665791775in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image30.tmp){width="5.203162729658793in"
height="3.770860673665792in"}

![A diagram of weights and multiplying Description automatically
generated](../../images/media2/image31.tmp){width="6.651090332458443in"
height="3.166689632545932in"}

So the code for this is actually really short and elegant. We\'ll start
with this DSC scoring process by passing in our decision context. And
this bonus, which is actually a combination of the weight that we just
talked about and a momentum factor, which is how likely am I to want to
change my mind. There\'s also some other factors that we\'ll get into
later, but essentially what we do is we start with a running counter,
which is set to the maximum value that that bonus could give us. If
everything is one, this is what we\'ll end up with. Then we loop
through. Our considerations. One at a time and actually score them
individually. The response? And then multiply that running counter by
the clamped value of that response score. So as we\'re going, we get the
final result and we can just return that as soon as we\'re done with all
the considerations.

![A screenshot of a computer program Description automatically
generated](../../images/media2/image32.tmp){width="6.671923665791776in"
height="3.1718985126859143in"}

So those are the two types of dses, the skill and the non skill, and
naturally they go into two types of containers. We call 1A decision
maker and one a skill set. And they\'re very similar. In some ways.
They\'re just a collection of DS es for evaluating actions, skill sets.
Of skilled. They\'re used to map the DSCS to the skills themselves,
whereas decision makers are collections of non skilled dses. It\'s
simple. List of individual dscs. It ends up being a list of all the non
skill actions that a character can take. So for an example of a skill
set, if we look at this human pirate here, he has four skills and four
evaluators that go with it. So for this pirate sword base slash basic
slash. We\'re going to use that common melee DSC that we\'ve seen a few
times to decide. Hey, is this a good idea for you to do at the time? On
the other hand, he has a pistol in his other hand. So he has this pistol
shot and we\'re just gonna use a common ranged DSC to do. Side is this
what you want to do? Looking at an inquest assassin. Very similar sort
of set. He\'s got his basic melee attack, but he also has a blinding
powder which is an area of effect that he can throw at range, and he has
common ranged AOE with D buff as. The the DSC that we\'re assigning to,
that same thing, he\'s. Some caltrops will drop at his feet and uses
that as a point blank area of effect. Now by just changing what\'s in
the right column there, we\'re going to change the expression. Of how
those characters are using their skills. By changing what\'s in. Box on
the decision maker side. As I mentioned, it really ends up being just an
entire list of all of the non skill actions that a character can take.
And we can add as many of them as we want. So we have in this guy, he\'s
got some tactical movement, he\'s got some emotes that he can perform.
He\'s got some ambient behaviors, like patrolling and wandering, and he
even has things like facing and looking at allies and enemies. It\'s
pretty much. That\'s his. That\'s the things that he can do in the
world.

![A white paper with black text Description automatically
generated](../../images/media2/image33.tmp){width="5.005244969378827in"
height="2.1770997375328083in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image34.tmp){width="5.348997156605424in"
height="3.1718985126859143in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image35.tmp){width="4.776076115485564in"
height="3.7239851268591426in"}

So to make a decision, all we have to do is go through a decision maker
and score everything that\'s in there. In this case, when we talk about
a decision, what we\'re referring to is a link to adse as well as a
target that may or may not be present. So we\'re going to go through
this list of decisions here that we\'ve been passed in and for each one
we just get the DSC. Run score function in the context that is relevant
for that target, and that\'s all we have to do. Now, as I\'m sure you
can imagine, at this point there\'s a lot of math going on here, and
that is driven home by the fact that we have hundreds of characters at a
time, each with dozens of DS es, each one that can be considered in
dozens of. At any given moment. So optimization was obviously very
important for us in this project. Thankfully, it was also very simple to
accomplish.

![A screenshot of a computer program Description automatically
generated](../../images/media2/image36.tmp){width="6.093794838145232in"
height="3.098980752405949in"}

![A white background with black text Description automatically
generated](../../images/media2/image37.tmp){width="2.7343952318460194in"
height="2.234390857392826in"}

So what we do is in those DSCS or in the decision makers rather DSCS are
sorted by weight with the highest weight at the top considerations
inside each DSC are also sorted by the likelihood to reduce the score.
Boolean things, items that could. Hey, when you get to this distance,
don\'t even think about doing this. Those are toward the top. Subtle
changes go towards the bottom. The only exception to this is Los checks
are last, since Ray casts are fairly expensive, we don\'t want to fire
off a raycast unless we\'re pretty sure that this DSC has a chance of
winning. So as we\'re going through and scoring all these, these are
discarded when we realize you can\'t possibly win, so we shouldn\'t
process anymore even if we\'re in the middle of the considerations
themselves. And so by sorting them like this, the most likely to succeed
actions are found first. And that really speeds up the rest of them a
lot of. Only brief checks are necessary, even just by checking the
weight that we\'re going to multiply it by, you can\'t win. On.

![A screenshot of a computer Description automatically
generated](../../images/media2/image38.tmp){width="6.4479636920384955in"
height="3.5000251531058617in"}

So if we go back to the DSC scoring code for a minute, you can see that
there\'s this parameter at the top called min. And what that basically
is is a watermark and you have to be at least above this watermark in
order to be relevant as a decision as we loop through the considerations
for the decision, all we have to do is check and see if our running
counter has dropped. That watermark, and if it has, we know that that
decision can\'t possibly win anymore. We can break out of the loop and
immediately discard the. See, we also do a number of other standard
optimizations. We have some limitations on context selection. There\'s a
range limit of about 4000 where you can\'t pay attention to anything
beyond your visibility limit. There\'s a hard cap of the number of
agents that you will even bother considering at one time. And we also do
a lot of standard tricks like memory caching or memory pooling and
caching for handling redundant queries for the same information.

![A screenshot of a computer program Description automatically
generated](../../images/media2/image39.tmp){width="6.260462598425197in"
height="3.151065179352581in"}

![A close-up of a person\'s face Description automatically
generated](../../images/media2/image40.tmp){width="3.1458562992125985in"
height="1.947930883639545in"}

So those are decision makers and skill sets and those two objects are
just put into what we call an intelligence definition. An intelligence
definition is what we\'re actually going to assign to the species in the
game. It contains a skill set, as I mentioned, a decision maker. Perhaps
some parameters for some of their behaviors that we can reference a
pathfinding preference profile, a habitat definition? That stuff. Not
going to get. It\'s actually for future work, but the point being is
that this is where it\'s going to contain all. All of the high level
information that we\'re going to assign to that species, this is how you
act. So looking at an intelligence definition for this, this guard here.
He has his. He has a decision maker and his skill set. He\'s pretty much
ready to go out into the world and he knows what it is that he can do.
So we also have what we call decision maker packages. Those are
supplemental decision makers that are in addition to what he\'s born
with. Already on the intelligence definition. And these situational
packages can be handed out by an event in the world by interacting with
an object. Or even by the map itself and dses that are in that get
processed right along with the dses that are in the core decision maker
for the species. So what I mean by that is, if we have an intelligence
definition and we have a decision maker with its DSCS, this is what the
character is born with. But we\'re not done there. We can, as I
mentioned, push decision makers onto that character and say here\'s some
more. We want you to consider and it would get considered right along
with everything else but the good news is is when we\'re done with that,
we can actually just pull those packages right back off and he goes back
to what he was originally born with to think about. So as an example of
this, let\'s say a villager goes to a Tavern because he it\'s tagged as
a social location and the Tavern has a trip wire that runs a script.
Says here, here\'s your Tavern behaviors. You\'re gonna need these. They
might include things like moving around between different interior
points of. May have a higher priority behavior for doing waves and look
ATS and cheers. But then one of his behaviors may very well be exit the
Tavern, and when he does so, he leaves the trip wire which runs his
script that says, you know what, you don\'t need those anymore out in
the real world. Take the Tavern behaviours right back off of you. And so
that way we have that sense of context of only use certain things when
you\'re in certain areas. You never even have to worry about.

![A white background with black text Description automatically
generated](../../images/media2/image41.tmp){width="3.6302351268591426in"
height="2.3020997375328083in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image42.tmp){width="5.890668197725284in"
height="3.067730752405949in"}

![A screenshot of a computer program Description automatically
generated](../../images/media2/image43.tmp){width="6.192753718285214in"
height="3.1458562992125985in"}

![A list of items on a white background Description automatically
generated](../../images/media2/image44.tmp){width="4.640659448818898in"
height="3.109397419072616in"}

So the second component we want to talk about is modular influence maps
influence maps. In a nutshell, for those of you who might not be
familiar with that, they\'re a representation of the world in terms of
the influence that an agent. Projects into it and it allows us to easily
aggregate influence from a lot of different sources, but process it once
for all agents to use. It eliminates a lot of the N \^2 problems we run
into, and a lot of the redundant calculation that goes with that. Now
uses for influence maps that we might have is information about a
specific location, like what\'s going on right here where I\'m standing,
or what\'s going on at a target that I\'m thinking about. You can also
use it for finding a location where should I move to right now. Where
should I target a particular spell? You can also just information about
your general area around you. Does a concentration of something exist?
Big is that concentration. I can just ask the IMAP system for that.

![A white paper with black text Description automatically
generated](../../images/media2/image45.tmp){width="3.9375284339457566in"
height="2.2812664041994752in"}

![A white text with black text Description automatically generated with
medium confidence](../../images/media2/image46.tmp){width="3.489608486439195in"
height="3.062521872265967in"}

So the system that we worked with is built out of two basic components.
The first is a knowledge representation layer, which is basically just
processing location and threat information about what is going on in the
world, and it gives us the ability to store and retrieve that inform. In
a very fluid way. The second component is modular construction, which is
actually much more interesting. We basically treat each layer of the
influence map system as an atomic component, and then we have algorithms
for shaping the way those are combined, and this assembles recipes. We
can use to build different kinds of information based on components that
are all fairly simple. So one of the the key factors for this is the
fall off function. If we\'re going to consider the proximity of an agent
to something, what we\'ll do is we\'ll center a point on the agent and
ask where can I move in about a second and for our. That comes out to
about a 250 inch radius. Linear fall off. Is this green line here that
you can see? And that\'s just saying as we get further away, drop off
the value towards 0. The threat map is something. We also center on the
agent, but instead of asking where can I move, we ask what could I
attack right now? And that gives us, on average about a 1200 radius in
our game at this point. So we use for this one an inverse quadratic
falloff with A4 exponent or A4 Poly. And you can see that illustrated by
the blue line. So to look at what this actually appears like in game,
there\'s an agent projecting some proximity influence around himself.
And you can see that it\'s bright at his feet and dims out and
disappears the further away you get. Now, as soon as we add a second
agent, you can see that where they overlap, there\'s an intense point.
That indicates that there\'s potentially a concentration of people in
that area, and of course, as we add more agents, these shapes can get
more complex and more interesting. Threat influence is of course very
analogous, except much wider in radius. So we can see this guy here
projecting out his large threat radius, and if we give him some friends,
you can tell that there\'s a lot of combined threat sitting in that
area. Very, very dense.

![A diagram of a diagram Description automatically
generated](../../images/media2/image47.tmp){width="4.322948381452319in"
height="3.703151793525809in"}

![A graph with a green arrow pointing to a graph Description
automatically
generated](../../images/media2/image48.tmp){width="6.484422572178477in"
height="3.098980752405949in"}

![A video game screen capture of a group of people walking on a sidewalk
Description automatically
generated](../../images/media2/image49.tmp){width="5.473998250218723in"
height="3.5521095800524933in"}

So we started with a grid based system. It was originally part of an
article that I wrote for the upcoming game AI Pro 2 we mentioned
earlier. Now this grid covered the entire game map in 100 inch grids and
we did one of these maps perfection so we could generate. Kind of idea
of allies and enemies. Excuse me and agents would stamp templates of
influence into the world for their faction. They would stamp a physical
one and they would stamp a threat, one like we talked about and now it
had some. However, we couldn\'t support a contextual agro system. And
what I mean by that is if I am neutral to you and you\'re neutral to my
friends etc. But I attack you. Well now. You hate me, but you\'re still
cool with my friends. And so by doing it with the. The factions that
didn\'t work so well. The other problem was that really this type of
system only worked well for for 2D. So what I mean by that is, if we
have the world we\'re looking at it from the side here and there\'s this
influence map that covers the entire world. Well, that\'s fine as agents
are moving into the world, they\'re projecting their influence into it.
Until that agent moves into, let\'s say. Tunnel well because we\'re
working with this one 2D map. He\'s also projecting this influence up to
the surface of this hill as well as the tunnel that he\'s in. So if an
enemy came along, he said, wait a minute, hold. There\'s enemy influence
here. But I can\'t see anyone. And so it\'s really confusing at that.
Point. So that\'s why we had to step away from that a little bit.

![A white paper with red text Description automatically
generated](../../images/media2/image50.tmp){width="4.562533902012248in"
height="3.093772965879265in"}

So what we ended up doing was embracing a system called the Infinite
Resolution System, which I wrote about also in game AI Pro 2. The idea
is pretty. We\'re going to store all of our influence as a sparse set of
points, and each point has a falloff function. Just like before. The
difference is instead of discretizing that into a grid, what we do is we
sum the falloff functions for the relevant nearby sources every time. Do
a query. We can also do minimum and maximum queries on this influence
map using gradient descent or hill climbing. For optimization purposes,
we only do this about once a second in our game right now, and the
points are actually stored in AKD tree data structure. In order to speed
up the searching for relevant influence sources. So as an example, what
this looks like if we have a. Location in the world. We want to know
what\'s the influence here? All we have to do is find the list of nearby
points that might possibly contribute influence, and then use that
falloff function to compute based on the distance between the query and
the source. What can contribution does each source have to the query
location? And when we\'re done, we just sum those up and that\'s our
result. Now, in order to find the best point, things get a little
messier and I\'ll skip the calculus for you. But basically. If we\'re
standing on this dotted line as an agent and we want to know where the
high point is, we\'re going to do a little bit of hill climbing and
essentially look at where can I find in a constantly increasing value in
the influence until I locate a. Where it\'s peaked. So this comes with a
couple of pros and cons. On the positive side. There\'s a lot less
memory usage because we don\'t have to store these massive grids.
Perfection for every map. It also handles the 3D environment problem
much more effectively, and it enables the complex relationships that
Dave was talking about. On the negative side, the queries become a lot
more expensive, especially when you want to do a minimum or maximum
query because of all the math that\'s involved and the code that
implements all this loses a lot of clarity and readability.

![A white text on a white background Description automatically
generated](../../images/media2/image51.tmp){width="5.125037182852143in"
height="3.3646073928258966in"}

![A diagram of a network Description automatically
generated](../../images/media2/image52.tmp){width="6.515672572178477in"
height="3.067730752405949in"}

![A graph showing the best point Description automatically
generated](../../images/media2/image53.tmp){width="4.875036089238845in"
height="3.0416885389326334in"}

![A white background with black text Description automatically
generated](../../images/media2/image54.tmp){width="5.963584864391951in"
height="2.4791852580927385in"}

So the Mike talked earlier about doing modular construction of these
results and this is the part from that article I wrote that we did keep.
What we do is we still have the idea of multiple layers of allies versus
enemies, physical versus threat, etcetera. And we can combine parts of
these layers together. To result in a lot of different expressions of
what\'s going on in the battlefield. We also have what we call a
personal interest template that prioritizes influence map values towards
what I would be most interested in, and so showing you that we\'re going
to say something that\'s centered on me and it starts at one at my
location and uses one of those. Off functions to go to 0 at maximum
range and so we\'re going to multiply any of the influence map scores
that we find. By that falloff function around me to call less
interesting. Certainly anything outside of that range is going to be
multiplied by zero, so it\'s not important. So looking again at that
chart, if this is the actual high point of some information around me.
And this is my fall off function that we\'re going to be multiplying it
by. You multiply those two curves and you see you get this. Notice that
the curve has. Well, that original high point has actually been squished
a little bit because it was out towards the edge of my personal interest
template. It was getting multiplied by a smaller value. So instead of
being interested in that point, which was the high one, I\'m going to be
interested in this one because yeah, they were similar, but this one\'s
kind of right here. And that one was farther away. So that\'s what the
the personal interest template gets us. So we can use influence maps for
location. We can query the IMAP system about a specific location like
say, what is the enemy proximity there? Is the enemy threat there? The.
Proximity. Are there environmental hazards that are radiating into the
influence map there? Going to just return a value about that location.
Because we\'re going to be aggregating all the info from the relevant
maps that we put into that type of query, we can combine them. As I
mentioned, sometimes they\'ll have weights. Might prioritize one over
the other. Now the locations of course can different. Can be asking
about. My location, am I crowded or what\'s going on by that target? He
alone, or does he have friends next to him? So as an example of this, we
have a DSC in the game evade dangerous melee situations, which is whoa
hey, too many dudes. I\'m going to back out one of the considerations
is, of course, well, when I\'m really close to enemies and the input
that we\'re using for that is influence enemy proximity. Just what it
sounds. It\'s the total value of the enemy proximity at my location
right now, and of course, as that increases, I\'m going to be less
enthusiastic about staying here. Desire to evade is going to increase.
Another one is we have closed to melee range from long range.
Consideration is not when there\'s already enough targets or. Excuse me
around the target and we\'re going to use influence, Ally. Proximity at
Target. It\'s exactly what it sounds. The input that we\'re getting. So
we\'re just totalling up the the ally proximity at that location. And as
that increases, our desire to close the target is going to decrease.
They can handle. I\'ll. I\'ll pick something else to do. So we can also
use influence maps for targeting information like retrieving a location
from the influence map that matches some criteria like. Is there a
concentration? Is there a sparseness? There an area of conflict where
two sides are both right there and we\'re going to return a location.
With the highest or lowest value as Mike was talking about with our hill
clim. Climbing, also subject to my personal interest template that is
the best match for a criteria, so I can say what\'s the safest place for
me to move to or where should I target that area of affect spell. So as
an example of this, we have ads that\'s main. Weapons distance and it
triggers the behavior. Stay at range. Will stay at range, she. Well,
where is the lowest enemy threat? That\'s still within my threat range.
Also, within my interest template, we have a variation on that as an
example that stay at range with spacing. And that\'s looking for the
lowest enemy threat and a couple other. Same as above, but notice that
we\'ve added one extra item to the recipe. That\'s the lowest ally
proximity, so we\'re staying away from enemy threats. Someplace close to
us, but we\'re also looking for a place that is not crowding our allies
just by adding one component to the influence map recipe. So those two
things, what they look like is this the that char there he has the
behavior. Stay at ranged weapons distance. Notice that if I get close to
him, he\'s like. Nope, Nope, Nope, Nope. Going to move over here. He
moves a little bit. He is looking for a point that has that is close to
him, that has the lowest enemy proximity \'cause. He doesn\'t want to be
close. Know you turn on the influence map debug. Notice what he\'s doing
is he\'s just going to walk outside of my my proximity there. He just
finds a close place that he can go to that matches that criteria. Now,
on the other hand, if we were to add the width spacing thing, you\'ll
see a completely different sort of thing. These guys are not only
staying away from me, but they\'re also picking locations that are not
next to each other. And so we end up with this really kind of cool
circle. Its worst firing squad ever. But notice that there are going to
be times as I try to move towards them that they will actually run past
me. Me because they can\'t get farther away without crowding their. So
like right here at the end, he\'s like, well, I can\'t move back.
\'cause my friends are there. So I\'m gonna move to the side. He\'s
trying to find a place in the influence map that matches both the
criterias. Stay away from the enemies, but leave spacing for my allies.
And that\'s just by adding one extra layer to that influence. Another
targeting example is of course doing a ranged area of effects. Well,
we\'re going to check for an enemy concentration to see if one exists to
begin with, and if there is one, we\'re going to want to use an area of
effects. Bell, right? But more importantly, we\'re going to get the
location of. The highest enemy proximity that\'s still within my
interest range. Is that concentration? So if there was a whole bunch of
enemies scattered around, I say, hey, is there a concentration? Look,
there\'s one, right? It\'s greater than 1.5. Really want to cast. This
big explosion spell, right? So we\'re going to actually target it on
that center of mass of the enemies right there to be able to encompass a
lot of them in my area of effect spell. Instead of casting an area of
effect on a specific person, we\'re saying where\'s the place to cast
it? To blow a lot of them up, because that\'s fun.

![A white paper with black text Description automatically
generated](../../images/media2/image55.tmp){width="5.9792104111986in"
height="2.4427263779527557in"}

![A diagram with a blue line Description automatically
generated](../../images/media2/image56.tmp){width="6.270879265091864in"
height="3.2968996062992124in"}

![A diagram of a map Description automatically
generated](../../images/media2/image57.tmp){width="6.505255905511811in"
height="3.276065179352581in"}

![A white paper with black text Description automatically
generated](../../images/media2/image58.tmp){width="6.364630358705162in"
height="2.2291830708661418in"}

![A screenshot of a computer Description automatically
generated](../../images/media2/image59.tmp){width="6.463589238845144in"
height="2.578144138232721in"}

![A white paper with red text Description automatically
generated](../../images/media2/image60.tmp){width="5.229204943132109in"
height="2.963562992125984in"}

![A white paper with black text Description automatically
generated](../../images/media2/image61.tmp){width="3.9427373140857394in"
height="3.130230752405949in"}

So the code for this in the grid version is actually really clean and
elegant. You can see here that in order to get that AOE target location.
All we have to do is from the code side is add the enemy locations up
for the relevant faction. Stamp out that personal interest template and
then find the location of the highest value just by searching the grid
linearly. The infinite resolution version is maybe superficially
similar, but unfortunately much more messy. The telltale sign here is
this function name. Optimization, highest within interest radius. Which
is a bit of a mouthful, but not quite as bad as the actual
implementation of that function, which is crammed on this slide for.
Enjoyment. That\'s that\'s not exactly readable so.

![A computer code on a white background Description automatically
generated](../../images/media2/image62.tmp){width="5.760459317585302in"
height="3.411482939632546in"}

So the third thing we wanted to talk about was the use of context.
Excuse me, content. Tags in our system. Now a lot of things can be. We
can tag characters either at the species level or an individual like
this guy right here. Not you, mark. We can tag gadgets or objects that
are in the world. Either all of them of one type or again a specific
one. One and we can tag locations like points of interest now. Oh, I\'m
sorry.

Go ahead. So these things can appear in the system from a number of
different origins. Basic content which is just all static predefined
definition of what gets tagged how. There\'s also the ability for
scripts in the game engine to. Have tags that are added and removed
dynamically. The utility AI actually also has a decision which allows us
to modify tags dynamically. And there\'s of course always the
possibility of arbitrary code in the game just doing what it wants to
with. For whatever reason, we come up with the way we do this is by
aggregating all of this tag information onto a blackboard type data
structure. So anything that\'s in the same map or section of the world
that you\'re in? Is able to read and write your data off your
blackboard, and that includes your. So some of the things that do read
tags are this get bonus factor function, which we\'ll talk about shortly
that checks tags when scoring DSE. And Dse\'s can also filter out
context completely based on the presence or absence of A tag. And then
scripts can do arbitrarily complex logic based on tags as well.

![A white paper with black text Description automatically
generated](../../images/media2/image63.tmp){width="4.854201662292214in"
height="3.26044072615923in"}

![A white background with black text Description automatically
generated](../../images/media2/image64.tmp){width="5.609416010498688in"
height="3.3021073928258966in"}

So content tags can be used in a lot of different. We can use it for
filtering things, so we can only search for context to have a matching
tag, for example, like so villagers that are wandering around may only
do so to locations that are tagged as poi. A point of interest. The
other hand, the soldiers. May have a behavior that they only wander
around between something that\'s tagged as patrol points. We can also
use them to qualify or disqualify. Don\'t use Firestorm on a creature
tagged as being fire based. Don\'t add fuel to it, so to speak. Or we
could have the soldiers saying we\'ll only salute. Other soldiers that
have officer as one of their tags, and so that behavior is only. In a
fire in the context of talking about another officer, we can also adjust
the scores of behaviors either specifically, say, increase the utility
of using ice storm on the creatures that are tagged Firebase, or we can
do it in a generic fashion. I mean by that is we have what we call
priority boost and cut tags. Built into every combat DSC, whether it be
a skill or. Legal movement behavior. And they adjust the final score of
that DSC. Can either be situational or event driven. For example, a
leader creature may point out a preferred target and place A tag on him
and all his minions go oh OK, that\'s what you want me to do. And they
increase the desire to go and attack that guy. Or we can do event
specified things where an event says for the duration of this event.
This thing right here is very important, more so than it normally is.
Actually have 8 layers of. We have 4 boosts and four cuts. That we
multiply by that weight that you see. So if the if the leader said this
guy right here is who I want you to attack, I\'m going to place priority
boost a large on him. All of the minions around there are gonna go. Oh,
everything that we do thinking about him, we\'re gonna multiply it by
1.5. He\'s gonna get a 50% bonus.

![A list of different types of tags Description automatically
generated](../../images/media2/image65.tmp){width="6.265670384951881in"
height="2.75001968503937in"}

The last thing we\'re going to touch on isn\'t really part of the
architecture, but we found that it was really important for making the
characters a little bit more believable as we tapped into a little bit
more of the IK head look. Now IK head look was already. The game, but it
was only on player rig characters. But we realized that we. It was a
concept that we could strip out and generalize fairly easily so we could
put it on other creatures and then hook it up with a variety of
different look at behaviors so they could. They could look at their
friends, they could look at enemies in comb. They could look at points
of interest. In the world, etc. And it really gave a lot of extra life
to the characters to make him feel like they were living in their own
world. So for example, here me and my buddy, we\'re going to go ahead
and attack this lady here. And if you look at her head, she\'s not
moving her feet, but you\'ll see her glance back and forth at the two of
us. That was not there before and so. You feel like I\'m deciding which
of the two of you I want to engage. Or I am wary about the the two of
you. If either of you going to try to attack. And so there\'s a lot of
this that we\'re trying to to work in that really makes those characters
feel a lot more alive and less. Robotic. So none of this matters unless
we can actually build a creatures AI out of it, of course. And yes, we
can. The walk through you can probably put it together a little bit by
now as we create a skill set, assign the skills to DS Es, we create a
decision maker and Add all the non skilled decisions. We create an
intelligence deaf and assign a skill set and a. Maker to that and then
we assign the intelligence definition to a species. Yes, it is actually
that. Because what we\'ve done is we created a lot of building blocks.
We actually have a library of. Sees sitting out there that are designed
to work together. We actually have hundreds of them that are pre created
that you can use to manipulate and create a character. A lot of them are
variations of the same thing for subtlety. So yeah, we have stay away
from enemy threats. A base version, but we also have a Conservative
version and a brave version and so depending on the type of character,
you\'re. Well, you can just select one of these to get a different feel.
If he\'s not feeling right, you could change it. Him more conservative.
Him braver. Some of them are specialized versions of other ones, so we
have closed melee range certainly, but we also have closed to melee
range from behind or sides only. Well, if you\'re making some sort of
assassin or ninja or cowardly guy, you give him that one. If you turn
your back on him, then he\'s going to come up on you. Just by choosing
the DS es from the library you can get a lot of different behavior. The
other thing too is of course, all the designers can add new ones and
they are free to. For everybody else, we\'re expanding that library. The
other thing we do is we have templates that are pre created decision
makers that have all the DS es required for certain basic behaviors. And
we have a number of different starter archetypes that we\'ve created. A
standard melee character. A melee, a character that enjoys attacking.
He\'s got different DS es for that, a stealth one. Different ranged
attackers. We have a whole bunch of different templates. You can
actually either link directly to them from the intelligence definition,
or you can clone it out and customize it. A good starter package I want
kind of this, but then I\'ll tweak it. So the bottom line though, is
because it\'s so easy to use. If a character exists, it has its skills
and its animations that have already been created by the creature
designers. You can create the unique AI for a brand new creature in 7
minutes. Yes, unique AI. You can choose what to put on these things in 7
minutes and to prove the point I actually did a time lapse video of
this. This is I\'m doing. You know a sample character. So as I crank
through this first thing I\'m do is do a skill set. This particular
character had four skills. I filled them out. I assigned them to the
evaluators that I wanted. I\'m creating a decision maker and I\'m saying
off the top of my head. Well, he needs to be able to do this and this
and this for tactical movement. Here\'s a bunch of emotes that he\'s
going to do. Here\'s some other animations and idle behaviors that he\'s
going to do. And so when I get done with that, I am actually going to
sort it member for the optimization we talked about earlier. So I\'m
just going to move some of the higher priority things to the. I actually
did second guess myself up here and I change a couple of these to
different variations of the behaviors. I assigned the skill set in the
decision maker to the intelligence staff. Put a little description in on
this guy. And we\'re done. 7 minutes to create the AI for one of the
hundreds of creatures in the game. Now of course, we can go farther in
depth than that. If we wanted to, but the point is it\'s that simple
because the architecture being modular and designed to work together
like that. So of course we have this. We had a mandate that we started
with Mike. How do you think we did?

Well, let\'s take a look from the AI execution side. We\'re definitely
using our skills better. We have all these DS, es and combinations of
inputs to choose from to decide how and when to use skills. Our movement
is vastly improved by the fact that we\'re using the influence maps the
way we are, and of course environmental awareness comes from both
influence maps and the tagging system. We also have the capacity for
building out that variety of behaviours just by recombining the building
blocks of Dave was talking about. We didn\'t get into it in too much
detail, but we have actually solved most of the exploits and believe it
or not, we\'re actually doing this slightly more slightly faster than
the existing AI for an order of magnitude more processing of possible
actions.

And So what he means is the fact that these characters, the new ones. Do
vastly more intelligence. Stuff in about the same processing time, then
in the the prior iteration. The other side of the equations we talked
about, the designer quality of. Yes, we are absolutely supporting a lot
more actions that the designers can work with and because of the utility
based system, the behavior can be far more autonomous. It doesn\'t get
into those brittle things that that will break down because of a of a
corner in the script, some place and as I just showed you for the
designer standpoint, there is a lot faster development. Time that
they\'re working with, so I that\'s a lot of green check marks. I think
we did fairly good fairly well. Me. I didn\'t do. Don\'t tell my wife I
correct her all the time anyway. The thing is, Mike and I have had a
blast over the past year working on. It\'s been a fantastic project. The
thing is, it wasn\'t just us. Mike and I wanted to thank a lot of people
from the executive level at Arena net. Very supportive. They trusted
Mike. To kind of go crazy with. And Mike brought me in about a year ago,
as I mentioned, and they let us say do what it is you need to do. See
where this goes? And that\'s a really nice attitude to get from from
executives. And the other thing too. Is. We weren\'t alone.

Absolutely. We have a lot of people to thank on the team that did things
ranging from building cortex for us to designing prototypes for all the
crazy ideas we kept coming up with. We had a lot of creature design that
was done. Hand in hand with this project so that we could make sure that
they were getting the most they could out of the work we were doing and
the content team has started picking this up and it\'s been a phenomenal
experience watching everybody on the on the game team. Just start to get
a feel for it and really warm up to it and then become excited about
using it and making it their own.

Yeah, I think that\'s probably one of the best things that Mike and I
got out of the project is once we started putting it in their hands and
not only them embracing what it is that we had already provided them,
but then saying, hey, well, does that mean? Can do this and this and
this. And in some cases, they could actually go ahead and do it on their
own because we had given the tools to them to work with. And so All in
all, it\'s been a fantastic experience and we\'re really looking forward
to continuing working on this architecture down the road. So we have 3
minutes left for questions approximately. So in the meantime, watch some
cool video. By the way, there\'s at least three or 400 characters that
will be in this. And we\'re under our processor budget. So thank you.

Yeah.

As always. As always, there are 4 microphones. If you have questions,
please use those.

Huh.

I am plugged in. Oh, I have to plug the plug into the plug. There we go.
Now I\'m good. So the server processor was fine. It was beating the snot
out of my laptop. How\'s that? Any questions? Yes.

When you\'re talking about your ranking, your you use the word implicit
versus explicit ranking, correct multiplying rather than adding that
that bonus did you explore the other way? Did you prefer what you did?
See what I\'m. Or did that just come? Of what you.

Well, part of it is the only way we could have done it where we had
explicit ranking. Saying for example, check all your emergencies first,
then if none of those fire, check all of the combat. Check all of
detective movement. Problem is is because all of those scores are, you
know, fluctuating. Could have a situation where something that is. A two
way a tactical movement because it had a high score multiplied by two
scores above. Of a combat thing that was a weight of 3. What was they
scored at 0.6? And so there\'s a lot of times that ebb and flow where,
yeah, there\'s combat things we could do, but there\'s this one tactical
movement thing that scored 100 percent 1.0 \* 2, and it got bumped up
above it. So. It is is you end up. This is a row obviously not going to
show up real well on the video is we have these overlapping ranges.
Where things can can can ebb and flow between them, whereas if we had
strictly stratified it, like in a behavior tree if. You\'re processing
this branch 1st, and if nothing fires in that, only then will you go to
the next branch. That wouldn\'t be. And so there\'s a lot of extra
fluidity that happens by using that system.

Right. If I can ask really quick, did that that overlapping did that
cause many problems? I\'ve had problems with that in the past.

Yeah, go ahead.

Wondering if. Guys had actually, I would.

I would venture to say that in a lot of cases it solved problems like
with the brittleness for example. It\'s like, well, you know, really you
should have done that. It was disqualified because you had some. For
scoring but higher priority things to do here and so it allows things to
really get itself out of corners fairly well. Not physically corners,
but as far as logically OK.

Good. Thanks.

Go ahead.

Thank. That\'s a very impressive presentation. I have two questions. The
first one is about how many times do you have on server to run this
massive amount of.

So that\'s, that\'s actually an entire lecture unto. But if you have GDC
Vault access, you can go back a couple years and view my presentation
here about that exact subject, but to give it a really quick summary,
imagine about four or five Atari 26 hundreds. OK, that\'s what you get
per character.

OK.

What the hell kind of unit is an Atari 2600? That\'s not scientific. We
actually are zilched on time and so you can feel free to corner one of
us afterwards. Sorry to cut you off on the question. I want you had a
second one too, where I have CAS that are threatening to throw things
at. All right, so.

So very quick.

No, no. What I meant was, I\'m sorry, we. We have to go ahead real quick
because this is also one of the reasons is we have a 15 minute turn
around for this next lecture.

OK.

It\'s about procedurally generated content. A lot of fantastic speakers
who did a joint lecture for us. So 15 minutes, folks. You back here for
that? Thank you.

Yeah.

