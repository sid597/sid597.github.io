My way to document things I implemented or find interesting for my future self and others.

Learning a lot from free resources can't match their quality yet, but this is my way of telling what I did and how I did it. Maybe someone can learn from this or help me get better by providing feedback. I know everything I have done can be done in a better way but as a self taught I don't know people whom I can ask for feedback and improve. So I am trying this form of documenting things and let people better than me provide feedback so I can improve and as a result help others improve by providing back from what I learned.

all right let's get started

this is 682 for distributed systems so

I'd like to start with just a brief

explanation of what I think a

distributed system is you know the core

of it is a set of cooperating computers

that are communicating with each other

over networked to get some coherent task

done and so the kinds of examples that

we'll be focusing on in this class are

things like storage for big websites or

big data computations such as MapReduce

and also somewhat more exotic things

like peer-to-peer file sharing so

they're all just examples the kinds of

case studies we'll look at and the

reason why all this is important is that

a lot of critical infrastructure out

there is built out of distributed

systems infrastructure that requires

more than one computer to get its job

done or it's sort of inherently needs to

be spread out physically so the reasons

why people build this stuff so first of

all before I even talk about distributed

systems sort of remind you that you know

if you're designing a system redesigning

you need to solve some problem if you

can possibly solve it on a single

computer you know without building a

distributed system you should do it that

way and there's many many jobs you can

get done on a single computer and it's

always easier so distributed systems you

know you should try everything else

before you try building distributed

systems because they're not they're not

simpler so the reason why people are

driven to use lots of cooperating

computers are they need to get

high-performance and the way to think

about that is they want to get achieve

some sort of parallelism lots of CPUs

lots of memories lots of disk arms

moving in parallel another reason why

people build this stuff is to be able to

tolerate faults

have two computers do the exact same

thing if one of them fails you can cut

over to the other one another is that

some problems are just naturally spread

out in space like you know you want to

do interbank transfers of money or

something well you know bank a has this

computer in New York City and Bank B as

this computer in London you know you

just have to have some way for them to

talk to each other and cooperate in

order to carry that out so there's some

natural sort of physical reasons systems

that are inherently physically

distributed for the final reason that

people build this stuff is in order to

achieve some sort of security goal so

often by if there's some code you don't

trust or you know you need to interact

with somebody but you know they may not

be immediate malicious or maybe their

code has bugs in it so you don't want to

have to trust it you may want to split

up the computation so you know your

stuff runs over there and that computer

my stuff runs here on this computer and

they only talk to each other to some

sort of narrow narrowly defined network

protocol assuming we may be worried

about you know security and that's

achieved by splitting things up into

multiple computers so that they can be

isolated the most of this course is

going to be about performance and fault

tolerance although the other two often

work themselves in by way of the sort of

constraints on the case studies that

we're going to look at you know all
these distributed systems so these
problems are because they have many
parts and the parts execute concurrently
because there are multiple computers you
get all the problems that come up with
concurrent programming all the sort of
complex interactions and we're
timing-dependent stuff and that's part
of what makes distributed systems hard
another thing that makes distributed
systems hard is that because again you
have multiple pieces plus a network you
can have very unexpected failure
patterns that is if you have a single
computer it's usually the case either

computer works or maybe it crashes or

suffers a power failure or something but

it pretty much either works or doesn't

work distributed systems made up of lots

of computers you can have partial

failures that is some pieces stopped

working other people other pieces

continue working or maybe the computers

are working but some part of the network

is broken or unreliable so partial

failures is another reason why

distributed systems are hard and a final

reason why it's hard is that you know

them the original reason to build the

distributed system is often to get

higher performance to get you know a

thousand computers worth of performance

or a thousand disk arms were the

performance but it's actually very

tricky to obtain that thousand X speed

up with a thousand computers there's

often a lot of roadblocks thrown in your

way so the Elven takes a bit of careful

design to make the system actually give

you the performance you feel you deserve

so solving these problems of course

going to be all about you know

addressing these issues the reason to

take the course is because often the

problems and the solutions are quite

just technically interesting they're

hard problems for some of these problems

there's pretty good solutions known for

other problems they're not such great

solutions now distributed systems are

used by a lot of real-world systems out

there like big websites often involved

you know vast numbers computers that are

you know put together as distributed

systems when I first started teaching

this course it was distributed systems

were something of an academic curiosity

you know people thought oh you know at a

small scale they were used sometimes and

people felt that oh someday they'd be

might be important but now particularly

driven by the rise of giant websites

that have you know vast amounts of data

and entire warehouses full of computers

distributed systems in the last

twenty years have gotten to be very

seriously important part of computing

infrastructure this means that there's

been a lot of attention paid to them a

lot of problems have been solved but

there's still quite a few unsolved

problems so if you're a graduate student

or you're interested in research there's

a lot to you let a lot of problems yet

to be solved in distributed systems that

you could look into his research and

finally if you like building stuff this

is a good class because it has a lab

sequence in which you'll construct some

fairly realistic distributed systems

focused on performance and fault

tolerance

so you've got a lot of practice building

districts just building distributed

systems and making them work all right

let me talk about course structure a bit

before I get started on real technical

content you should be able to find the

course website using Google and on the

course website is the lab assignments

and course schedule and also link to a

Piazza page where you can post questions

get answers the course staff I'm Robert

Morris I'll be giving the lectures I

also have for TAS you guys want to stand

up and show your faces the TAS are

experts at in particular at doing the

solving the labs they'll be holding

office hours so if you have questions

about the labs you can come you should

go to office hours or you could post

questions to Piazza the course has a

couple of important components one is

this lectures there's a paper for almost

every lecture there's two exams

there's the labs programming labs and

there's an optional final project that

you can do instead of one of the labs

the lectures will be about two big ideas

in distributed systems they'll also be a

couple of lectures that are more about

sort of lab programming stuff a lot of

our lectures will be taken up by case

studies a lot of the way that I sort of

try to bring out the content of

distributed systems is by looking at

papers some academics some written by

people in industry describing real

solutions to real problems

these lectures actually be videotaped

and I'm hoping to post them online so

that you can so if you're not here or

you want to review the lectures you'll

be able to look at the videotape

lectures the papers again there's one to

read per week most of a research paper

some of them are classic papers like

today's paper which I hope some of you

have read on MapReduce it's an old paper

but it was the beginning of its spurred

an enormous amount of interesting work

both academic and in the real world so

some are classic and some are more

recent papers sort of talking about more

up-to-date research what people are

currently worried about and from the

papers we'll be hoping to tease out what

the basic problems are what ideas people

have had that might or might not be

useful in solving distributed system

problems we'll be looking at sometimes

in implementation details in some of

these papers because a lot of this has

to do with actual construction of of

software based systems and we're also

going to spend a certain time looking at

evaluations people evaluating how fault

tolerant their systems by measuring them

or people measuring how much performance

or whether they got performance

improvement at all so I'm hoping that

you'll read the papers before coming to

class the lectures are maybe not going

to make as much sense if you haven't

already read the lecture because there's

not enough time to both explaining all

the content of the paper and have a sort

of interesting reflection on what the

paper means online class so you really

got to read the papers before I come

into class and hopefully one of the

things you'll learn in this class is how

to read a paper rapidly in the fish

and skip over the parts that maybe

aren't that important and sort of focus

on teasing out the important ideas on

the website there's for every link to

buy the schedule there's a question that

you should submit an answer for for

every paper I think the answers are due

at midnight and we also ask that you

submit a question you have about the

paper through the website in order both

to give me something to think about as

I'm preparing the lecture and if I have

time I'll try to answer at least a few

of the questions by email and the

question and the answer for each paper

due midnight the night before there's

two exams there's a midterm exam in

class I think on the last class meeting

before spring break and there's a final

exam during final exam week at the end

of the semester the exams are going to

focus mostly on papers and the labs and

probably the best way to prepare for

them as well as attending lecture and

reading the papers a good way to prepare

for the exams is to look at all exams we

have links to 20 years of old exams and

solutions and so you look at those and

sort of get a feel for what kind of

questions that I like to ask and indeed

because we read many of the same papers

inevitably I ask questions each year

that can't help but resemble questions

asked in previous years the labs there's

for programming labs the first one of

them is due Friday next week lab one is

a simple MapReduce lab to implement your

own version of the paper they write

today in which I'll be discussing in a

few minutes

lab 2 involves using a technique called

raft in order to get fault taught in

order to sort of allow in theory allow

any system to be made fault tolerant by

replicating it and having this raft

technique manage the replication and

manage sort of automatic cut

or if there's a field if one of the

replicated servers fails so this is rav4

fault tolerance in lad 3 you'll use your

raft implementation in order to build a

fault tolerant key-value server it'll be

replicated and fault tolerant in a lab 4

you'll take your replicated key-value

server and clone it into a number of

independent groups and you'll split the

data in your key value storage system

across all of these individual

replicated groups to get parallel

speed-up by running multiple replicated

groups in parallel and you'll also be

responsible for moving the various

chunks of data between different servers

as they come and go without dropping any

balls so this is a what's often called a

sharded key value service sharding

refers to splitting up the data

partitioning the data among multiple

servers in order to get parallel speed

up if you want instead of doing lab 4

you can do a project of your own choice

and the idea here is if you have some

idea for a distributed system you know

in the style of some of the distributed

systems we talked about in the class if

you have your own idea that you want to

pursue and you like to build something

and measure whether it worked in order

to explore your idea you can do a

project and so for a project you'll pick

some teammates because we require that

projects are done in teams of two or

three people so like some teammates and

send your project idea to us and we'll

think about it and say yes or no and

maybe give you some advice and then if

you go ahead and do if we say yes and

you want to do a project you do that and

instead of lab 4 and it's due at the end

of the semester and you know you'll you

should do some design work and build a

real system and then in the last day of

class you'll demonstrate your system

as well as handing in a short sort of

written report to us about what you

built and I posted on the website some

some ideas which might or may not be

useful for you to sort of spur thoughts

about what projects you might build but

really the best projects are one where

sort of you have a good idea for the

project and the idea is if you want to

do a project you should choose an idea

that's sort of in the same vein as the

systems that were talked about in this

class

okay back to labs the lab Greed's they

we give you you hand in your lab code

and we run some tests against it and

you're great early based on how many

tests you pass we give you all the tests

that we use those no hidden tests so if

you implement the lab and it reliably

passes all the tests and chances are

good unless there's something funny

going on which there sometimes is

chances are good that if you your coop

passes all the tests when you run it or

pass all the tests when we run it and

you'll get a four score full score so

hopefully there'll be no mystery about

what score you're likely to get on the

labs let me warn you that debugging

these labs can be time-consuming because

they're distributed systems and a lot of

concurrency and communication sort of

strange difficult to debug errors can

crop up so you really ought to start the

labs early don't don't even have a lot

of trouble if you be elapsed to the last

moment you got to start early if your

problems please come to the TAS office

hours and please feel free to ask

questions about the labs on Piazza and

indeed I hope if you know the answer

that you'll answer people's questions on

Piazza as well all right any questions

about the mechanics of the course yes

so the question is what is how does how

do the different factor these things

factoring the grade I forget but it's

all on the it's on the website under

something I think though it's the labs

are the single most important component

okay alright 

so this is a course about

about infrastructure for applications

and so all through this course there's

going to be a sort of split in the way I

talk about things between applications

which are sort of other people the

customer somebody else writes but the

applications are going to use the

infrastructure that we're thinking about

in this course and so the kinds of

infrastructure that tend to come up a

lot our storage communication and

computation and we'll talk about systems

that provide all three of these kinds of

infrastructure the the storage it turns

out that storage is going to be the one

we focus most on because it's a very

well-defined and useful abstraction and

usually fairly straightforward

abstraction so people know a lot about

how to build how to use and build

storage systems and how to build sort of

replicated fault tolerant

high-performance distributed

implementations of storage we'll also

talk about some some of our computation

systems like MapReduce for today is a

computation system and we will talk

about communications some but mostly

from the point is a tool that we need to

use to build distributed systems like

computers have to talk to each other

over a network you know maybe you need

reliability or something and so we'll

talk a bit about what we're actually

mostly consumers of communication if you

want to learn about communication

systems as sort of how they work that's

more the topic of six eight to nine

so for storage and computation a lot of

our goal is to be able to discover

abstractions where use of simplifying

the interface to these two storage and

computation distributed storage and

computation infrastructure so that it's

easy to build applications on top of it

and what that really means is that we

need to we'd like to be able to build

abstraction that hide the distributed

nature of these of these systems so the

dream which is rarely fully achieved but

the dream would be to be able to build

an interface that looks to an

application is if it's a non distributed

storage system just like a file system

or something that everybody already

knows how to program and has a pretty

simple model semantics we'd love to be

able to build interfaces that look and

act just like non distributed storage

and computation systems but are actually

you know vast extremely high performance

fault tolerant distributed systems

underneath so we both have abstractions

and you know as you'll see as a course

goes on we sort of you know only part of

the way there it's rare that you find an

abstraction for a distributed version of

storage or computation that has simple

behavior but he's just like the non just

non distributed version of storage that

everybody understands but people getting

better at this and we're gonna try to

study the ways and what people have

learned about building such abstractions

ok so what kind of what kind of topics

show up is we're considering these

abstractions the first one this first

topic general topic that we'll see a lot

a lot of the systems we looked at have

to do with implementation so for example

the kind of tools that you see a lot for

for ways people learn how to build these

systems are things like remote procedure

call

whose goal is to mask the fact that

we're communicating over an unreliable

Network another kind of implementation

topic that we'll see a lot is threads

which are a programming technique that

allows us to harness what allows us to

harness multi-core computers but maybe

more important for this class threads

are a way of structuring concurrent

operations in a way that's hopefully

simplifies the programmer view of those

concurrent operations and because we're

gonna use threads a lot it turns out

we're going to need to also you know

just as from an implementation level

spend a certain amount of time thinking

about concurrency control things like

locks and the main place that these

implementation ideas will come up in the

class they'll be touched on in many of

the papers but you're gonna come face

the face of all this in a big way in the

labs you need to build distributed you

know do the programming for distributed

system and these are like a lot of sort

of important tools you know beyond just

sort of ordinary programming these are

some of the critical tools that you'll

need to use to build distributed systems

another big topic that comes up in all

the papers we're going to talk about is

performance usually the high-level goal

of building a distributed system is to

get what people call scalable speed-up

so we're looking for scalability and

what I mean by scalability or scalable

speed-up is that if I have some problem

that I'm solving with one computer and I

buy a second computer to help me execute

my problem if I can now solve the

problem in half the time or maybe solve

twice as many problem instances you know

per minute on two computers as I had on

one then that's an example of

scalability so

sort of two times you know computers or

resources gets me you know two times

performance or throughput and this is a

huge hammer if you can build a system

that actually has this behavior Namie

that if you increase the number of

computers you throw at the problem by

some factor you get that factor more

throughput more performance out of the

system that's a huge win because you can

buy computers with just money right

whereas if in order to get the

alternative to this is that in order to

get more performance you have to pay

programmers to restructure your software

to get better performance to make it

more efficient or to apply some sort of

specialized techniques better algorithms

or something if you have to pay

programmers to fix your code to be

faster that's an expensive way to go

we'd love to be able just oh by thousand

computers instead of ten computers and

get a hundred times more throughput

that's fantastic and so this sort of

scalability idea is a huge idea in the

backs of people's heads when they're

like building things like big websites

that run on are you know building full

of computers if the building full of

computers is there to get a sort of

corresponding amount of performance but

you have to be careful about the design

in order to actually get that

performance so often the way this looks

when we're looking at diagrams or I'm

writing diagrams in this course is that

I'm not supposing we're building a

website ordinarily you might have a

website that you know has a HTTP server

let's say it has some types of users

many web browsers and they talk to a web

server running Python or PHP or whatever

sort of web server and the web server

talks to some kind of database

you know when you have one or two users

you can just have one computer running

both and maybe a computer for the web

server and a computer from the database

but maybe all of a sudden you get really

proper popular and you'll be up and

you've you know 100 million people sign

up your service ID how do you how do you

fix your c-certainly can it support

millions of people on a single computer

except by extremely careful

labor-intensive optimization but you

don't have time for so typically the way

you're going to speed things up the

first thing you do is buy more web

servers and just split the user so that

you know how few users or some fraction

the user go to a web server 1 and the

other half you send them to a web server

2 and because maybe you're building I

don't know what reddit or something

where all the users need to see the same

data ultimately you have all the web

servers talk to the backend and you can

keep on adding web servers for a long

time here and so this is a way of

getting parallel speed up on the web

server code you know if you're running

PHP or Python maybe it's not too

efficient as long as each individual web

server doesn't put too much load on the

database you can add a lot of web

servers before you run into problems but

this kind of scalability is rarely

infinite unfortunately certainly not

without serious thought and so what

tends to happen with these systems is

that at some point after you have 10 or

20 or 100 web servers all talking to the

same database now all of a sudden the

database starts to be a bottleneck and

adding more web servers no longer helps

so it's rare that you get full scale

ability to sort of infinite numbers of

adding infinite numbers of computers

some point you run out of gas because

the place at which you are adding more

computers is no longer the bottleneck by

having lots and lots of web servers we

basically moved the bottleneck

I think it's limiting performance from

the web servers to the database and at

this point actually you almost certainly

have to do a bit of design work because

it's rare that you can

there's any straightforward way to take

a single database and sort of refactor

things with it or you can take data

sorta in a single database and refactor

it so it's split over multiple databases

but it's often a fair amount of work and

because it's awkward but people many

people actually need to do this we're

gonna see a lot of examples in this

course in which the distributed system

people are talking about is a storage

system because the authors were running

you know something like a big website

that ran out of gas on a single database

or storage servers anyway so the

scalability story is we love to build

systems that scale this way but you know

it's hard to make it or takes work off

and design work to push this idea

infinitely far ok so another big topic

that comes up a lot is fault tolerance

if you're building a system with a

single computer in it well a single

computer often can stay up for years

like I have servers in my office that

have been up for years without crashing

you know the computer is pretty reliable

the operating systems reliable

apparently the power in my building is

pretty reliable so it's not uncommon to

have single computers it's just a for

amazing amount of time however if you're

building systems out of thousands of

computers then even if each computer can

be expected to stay up for a year with a

thousand computers that means you're

going to have like about three computer

failures per day in your set of a

thousand computers so solving big

problems with big distributed systems

turns sort of very rare fault tolerance

very real failure very rare failure

problems into failure problems that

happen just all the time in a system

with a thousand computers there's almost

certainly always something broken it's

always some computer that's either

crashed or mysteriously you know running

incorrectly or slowly or doing the wrong

thing or maybe there's some piece of the

network with a thousand computers we got

a lot of network cables and a lot of

network switches and so you know there's

always some network cable that somebody

stepped on and is unreliability or

network cable that fell out or some

networks which whose fan is broken and

the switch overheated and failed there's

always some little problem somewhere in

your building sized distributed system

so big scale turns problems from very

rare events you really don't have to

worry about that much into just constant

problems that means the failure has to

be really or the response the masking of

failures the ability to proceed without

failures just has to be built into the

design because there's always failures

and you know it's part of building you

know convenient abstractions for

application programmers we really need

that but to be able to build

infrastructure that as much as possible

hides the failures from application

programmers or masks them or something

so that every application programmer

doesn't have to have a complete

complicated story for all the different

kinds of failures that can occur there's

a bunch of different notions that you

can have about what it means to be fault

tolerant about a little more but you

know exactly what we mean by that we'll

see a lot of a lot of different flavors

but among the more common ideas you see

one is availability so you know some

systems are designed so that under some

kind certain kinds of failures not all

failures but certain kinds of failures

the system will keep operating despite

the failure while providing you know

undamaged service the same kind of

service it would have provided even if

there had been no failure so some

systems are available in that sense that

up and up you know so if you build a

replicated service that maybe has two

copies you know one of the replicas

replica servers fail fails maybe the

other server can continue operating

they both fail of course you can't you

know you can't promise availability in

that case so available systems usually

say well under certain set of failures

we're going to continue providing

service we're going to be available more

failures than that occur it won't be

available anymore

another kind of fault tolerance you

might you might have or in addition to

availability or by itself as

recoverability and what this means is

that if something goes wrong maybe the

service will stop working that it is

it'll simply stop responding to requests

and it will wait for someone to come

along and repair or whatever went wrong

but after the repair occurs the system

will be able to continue as if nothing

bad had gone wrong right so this is sort

of a weaker requirement than

availability because here we're not

going to do anything while while the

failed come until the failed component

has been repaired but the fact that we

can get up get going again without you

know but without any loss of correctness

is still a significant requirement it

means you know recoverable systems

typically need to do things like save

their latest date on disk or something

where they can get it back

you know after the power comes back up

and even among available systems in

order for a system to be useful in real

life usually what the way available

systems are SPECT is that they're

available until some number of failures

have happened if too many failures have

happened an available system will stop

working or you know will stop responding

at all but when enough things have been

repaired it'll continue operating so a

good available system will sort of be

recoverable as well in a sensitive to

many failures occur

it'll stop answering but then will

continue correctly after that so this is

what we love - this is what we'd love to

obtain the biggest hammer what we'll see

a number of approaches to solving these

problems there's really sort of

things that are the most important tools

we have in this department one is

non-volatile storage so that you know

something crash power fails or whatever

there's a building wide power failure we

can use non-volatile store it's like

hard drives or flash or solid-state

drives or something to sort of store a

check point or a log of the state of a

system and then when the power comes

back up or somebody repairs our power

suppliers notice what we'll be able to

read our latest state off the hard drive

and continue from there so so one tool

is sort of non-volatile storage and the

management of non-volatile storage just

Ning comes up a lot because non-volatile

storage tends to be expensive to update

and so a huge amount of the sort of

nitty-gritty of building sort of

high-performance fault-tolerant systems

is in you know clever ways to avoid

having to write the non-volatile storage

too much in the old days and even today

you know what writing non-volatile

storage meant was moving a disk arm and

waiting for a disk platter to rotate

both of which are agonizingly slow on

the scale of you know three gigahertz

microprocessors good things like flash

life is quite a bit better but still

requires a lot of thought to get good

performance out of and the other big

tool we have for fault tolerance is

replication and the management of

replicated copies is sort of tricky you

know that sort of he problem lurking in

any replicated system where we have two

servers each with a supposedly identical

copy of the system state the key problem

that comes up is always that the two

replicas will accidentally drift out of

sync and will stop being replicas right

and this is just you know with the back

of the every design that we're gonna see

for using replication to get fault

tolerance and lab - a lot - you're all

about management management of

replicated copies for fault tolerance

as you'll see it's pretty complex a

final topic final cross-cutting topic is

consistency so it's an example of what I

mean by consistency supposing we're

building a distributed storage system

and it's a key/value service so it just

supports two operations maybe there's a

put operation and you give it a key and

a value and that the storage system sort

of stashes away the value under as the

value for this key maintains it's just a

big table of keys and values and then

there's a good operation you the client

sends it a key and the storage service

is supposed to you know respond with the

value of the value it has stored for

that key right and this is kind of good

when I can't think of anything else as

an example of a distributed system all

Oh without key value services and

they're very useful right they're just

sort of a kind of fundamental simple

version of a storage system so of course

if you're an application programmer it's

helpful if these two operations kind of

have meanings attached to them that you

can go look in the manual and the manual

says you know what it what it means what

you'll get back if you call get right

and sort of what it means for you to

call put all right so it's immediate so

some sort of spec for what they meant

otherwise like who knows how can you

possibly write an application without a

description of what putting get are

supposed to do and this is the topic of

consistency and the reason why it's

interesting in distributed systems is

that both for performance and for fault

tolerant reasons fault tolerance reason

we often have more than one copy of the

data floating around so you know in a

non distributed system where you just

have a single server with a single table

there's often although not always but

there's often like relatively no

ambiguity about what pudding get could

possibly mean right in

to ative Lee you know what put means is

update the table and what get means is

just get me the version that's stored in

the table which but in a distributed

system where there's more than one copy

in the data due to replication or

caching or who knows what there may be

lots of different versions of this key

value pair floating around like if one

of the replicas you know if supposing

some client issues a put and you know

there's two copies of the the server so

they both have a key value table right

and maybe key one has value twenty on

both of them and then some client issues

a put nice we have client over here and

it's gonna send a put it wants to update

the value of one to be twenty-one all

right maybe it's counting stuff in this

key value server so sends a put with key

one and value twenty one it sends it to

the first server and it's about to send

the same put you know wants to update

both copies right it keeps them in sync

it's about to send this put but just

before it sends to put to the second

server crashes I power failure or bug an

operating system or something so now the

state were left in sadly is that we sent

this put and so we've updated one of the

two replicas didn't have value twenty

one but the other ones still with twenty

now somebody comes along and reads with

a get and they might get they want to

read the value associated with key one

they might get twenty one or they might

get twenty depending on who they talk to

and even if the rule is you always talk

to the top server first if you're

building a fault-tolerant system the

actual rule has to be oh you talk to the

top server first unless it's failed in

which case you talk to the bottom server

so either way someday you risk exposing

this stale copy of the data to some

future again it could be that many gets

get the updated twenty one and then like

next week all of a sudden some get

yields you know a week old copy of the

data so that's not very consistent

right so in order but you know it's the

kind of thing that could happen right

we're not careful so you know we need to

have we need to actually write down what

the rules are going to be about puts and

gets given this danger of due to

replication and it turns out there's

many different definitions you can have

of consistency you know many of them are

relatively straightforward many of them

sound like well I get yields the you

know value put by the most recently

completed put all right so that's

usually called strong consistency it

turns out also it's very useful to build

systems that have much weaker

consistency there for example do not

guarantee anything like a get sees the

value written by the most recent put and

the reason so there's there strongly

consistent systems they usually have

some version that gets seen most recent

puts although you have to there's a lot

of details to work out there's also

weekly consistent many sort of flavors

of weekly consistent systems that do not

make any such guarantee that you know

may guarantee well you're you know if

someone does a put then you may not see

the put you may see old values that

weren't updated by the put for an

unbounded amount of time maybe and the

reason for people being very interested

in wheat consistency schemes is that

strong consistency that is having Rezac

Chua lessee be guaranteed to see the

most recent right that's a very

expensive spec to implement because what

it means is almost certainly that you

have to somebody has to do a lot of

communication in order to actually

implement some notion of strong

consistency if you have multiple copies

it means that either the writer or the

reader or maybe both has to consult

every copy like in this case where you

know maybe a client crash left one

updated but not the other if we wanted

to implement strong

Sisseton see in them maybe a simple way

in this system we'd have readers read

both of the copies or if there's more

than one copy all the copies and use the

most recently written value that they

find but that's expensive that's a lot

of chitchat to read one value so in

order to avoid communication as much as

possible particularly if replicas are

far away people build weak systems that

might actually allow the stale read of

an old value in this case although

there's often more semantics attached to

that to try to make these weak schemes

more useful and we're this communication

problem you know strong consistency

requiring expensive communication where

this really runs you into trouble is

that if we're using replication for

fault tolerance then we really want the

replicas to have independent failure

probability to have uncorrelated failure

so for example putting both of the

replicas of our data in the same iraq in

the same machine room it's probably a

really bad idea

because if someone trips over the power

cable to that rack both of our copies of

our data are going to die because

they're both attached to the same power

cable in the same rack so in the search

for making replicas as independent and

failure as possible in order to get

decent fault tolerance people would love

to put different replicas as far apart

as possible like in different cities or

maybe on opposite sides of the continent

so an earthquake that destroys one data

center will be extremely unlikely to

also destroy the other data center that

as the other copy you know so we'd love

to be able to do that if you do that

then the other copy is thousands of

miles away and the rate at which light

travels means that it may take on the

order of milliseconds or tens of

milliseconds to communicate to a data

center across the continent in order to

update the other copy of the data and so

that makes this the communication

required for strong consistency for good

consistency potentially extremely

expensive like every time you want to do

one of these put opera

or maybe again depending on how you

implement it you might have to sit there

waiting for like 10 or 20 or 30

milliseconds in order to talk to both

copies of the data to ensure that

they're both updated or or both checked

to find the latest copy and that

tremendous expense right this is 10 or

20 or 30 milliseconds on machines that

after all I'll execute like a billion

instructions per second so we're wasting

a lot of potential instructions while we

wait people often go much weaker systems

you're allowed to only update the

nearest copy you're only consulted

nearest copy I mean there's a huge sort

of amount of academic and real-world

research on how to structure weak

consistency guarantees so they're

actually useful to applications and how

to take advantage of them in order to

actually get high performance alright so

that's a lightning preview of the

technical ideas in the course any

questions about this before I start

talking about MapReduce all right I want

to switch to Map Reduce that's a sort of

detailed case study that's actually

going to illustrate most of the ideas

that we've been talking about here now

produces a system that was originally

designed and built and used by Google I

think the paper dates back to 2004 the

problem they were faced with was that

they were running huge computations on

terabytes and terabytes of data like

creating an index of all of the content

of the web or analyzing the link

structure of the entire web in order to

identify the most important pages or the

most authoritative pages as you know the

whole web is what's even in those days

tens of terabytes of data building index

of the web is basically equivalent to a

sort running sort of the entire data

sort you know ones like reasonably

expensive and to run a sort on the

entire content to the way I've been a

single computer

how long would have taken but you know

it's weeks or months or years or

something so Google the time was

desperate to be able to run giant

computations on giant data on thousands

of computers in order that the

computations could finish rapidly it's

worth it to them to buy lots of

computers so that their engineers

wouldn't have to spend a lot of time

reading the newspaper or something

waiting for their big compute jobs to

finish and so for a while they had their

clever engineer or sort of handwrite you

know if you needed to write a web

indexer or some sort of Lincoln outlay a

blink analysis tool you know Google

bought the computers and they say here

engineers you know do write but never

whatever software you like on these

computers and you know they would

laborious ly write the sort of one-off

manually bitten software to take

whatever problem they were working on

and so to somehow farm it out to a lot

of computers and organize that

computation and get the data back if you

only hire engineers who are skilled

distributed systems experts maybe that's

ok although even then it's probably very

wasteful of engineering effort but they

wanted to hire people who were skilled

at something else and not necessarily

engineers who wanted to spend all their

time writing distributed system software

so they really needed some kind of

framework that would make it easy to

just have their engineers write the kind

of guts of whatever analysis they wanted

to do like the sort algorithm or a web

index or link analyzer or whatever just

write the guts of that application and

not be able to run it on a thousands of

computers without worrying about the

details of how to spread the work over

the thousands of computers how to

organize whatever data movement was

required how to cope with the inevitable

failures so they were looking for a

framework that would make it easy for

non specialists to be able to write and

run giant distributed computations and

so that's what MapReduce is all about

and the idea is that the programmer just

write the application designer

consumer of this distributed computation

I'm just be able to write a simple map

function and a simple reduce function

that don't know anything about

distribution and the MapReduce framework

would take care of everything else so an

abstract view of how what MapReduce is

up to is it starts by assuming that

there's some input and the input is

split up into some a whole bunch of

different files or chunks in some way so

we're imagining that no yeah you know

input file one and put file two etc you

know these inputs are maybe you know web

pages crawled from the web or more

likely sort of big files that contain

many web each of which contains many web

files crawl from the web all right and

the way Map Reduce

starts is that you're to find a map

function and the MapReduce framework is

gonna run your map function on each of

the input files and of course you can

see here there's some obvious

parallelism available can run the maps

in parallel so the each of these map

functions only looks as this input and

produces output the output that a map

function is required to produce is a

list you know it takes a file as input

and the file is some fraction of the

input data and it produces a list of key

value pairs as output the map function

and so for example let's suppose we're

writing the simplest possible MapReduce

example a word count MapReduce job goal

is to count the number of occurrences of

each word so your map function might

emit key value pairs where the key is

the word and the value is just one so

for every word at C so then this map

function will split the input up into

words or everywhere ditzies

it emits that word as the key and 1 as

the value and then later on will count

up all those ones in order to get the

final output so you know maybe input 1

has the word

a in it and the word B in it and so the

output the map is going to produce is

key a value one key B value one maybe

the second not communication sees a file

that has a B in it and nothing else so

it's going to implement output b1 maybe

this third input has an A in it and a C

in it alright so we run all these maps

on all the input files and we get this

intermediate with the paper calls

intermediate output which is for every

map a set of key value pairs as output

then the second stage of the computation

is to run the reduces and the idea is

that the MapReduce framework collects

together all instances from all maps of

each key word so the MapReduce framework

is going to collect together all of the

A's you know from every map every key

value pair whose key was a it's gonna

take collect them all and hand them to

one call of the programmer to find

reduce function and then it's gonna take

all the B's and collect them together of

course you know requires a real

collection because they were different

instances of key B were produced by

different indications of map on

different computers so we're not talking

about data movement I'm so we're gonna

collect all the B keys and hand them to

a different call to reduce that has all

of the B keys as its arguments and same

as C so there's going to be the

MapReduce framework will arrange for one

call to reduce for every key that

occurred in any of the math output and

you know for our sort of silly word

count example all these reduces have to

do or any one of them has to do is just

count the number of items passed to it

doesn't even have to look at the items

because it knows that each of them is

the word is responsible for plus one is

the value you don't have to look at

those ones we've just count

so this reduce is going to produce a and

then the count of its inputs this reduce

it's going to produce the key associated

with it and then count of its values

which is also two so this is what a

typical MapReduce job looks like the

high level just for completeness the

well some a little bit of terminology

the whole computation is called the job

anyone invocation of MapReduce is called

a task so we have the entire job and

it's made up of a bunch of math tasks

and then a bunch of produced tasks so

it's an example for this word count you

know the what the map and reduce

functions would look like the map

function takes a key in the value as

arguments and now we're talking about

functions like written in an ordinary

programming language like C++ or Java or

who knows what so this is just code

people ordinary people can write what a

map function for word count would do is

split the the key is the file name which

typically is ignored we really care what

the file name was and the V is the

content of this maps input file so V is

you know just contains all this text

we're gonna split V into words and then

for each word

we're just gonna emit and emit takes two

arguments mitts you know calmly map can

make emit is provided by the MapReduce

framework we get to produce we hand emit

a key which is the word and a value

which is the string one so that's it for

the map function and a word count map

function and MapReduce literally it

could be this simple

so there's sort of promise to make the

and you know this map function doesn't

know anything about distribution or

multiple computers or the fact we need

we need to move data across the network

or who knows what

this is extremely straightforward and

the reduce function for a word count the

reduce is called with you know remember

each reduce is called with sort of all

the instances of a given key on the

MapReduce framework calls reduce with

the key that it's responsible for and a

vector of all the values that the maps

produced associated with that key the

key is the word the values are all ones

we don't like here about them we only

care about how many they were and so

reduce has its own omit function that

just takes a value to be emitted as the

final output as the value for the this

key so we're gonna admit a length of

this array so this is also about as

simplest reduce functions have are and

in Map Reduce namely extremely simple

and requiring no knowledge about fault

tolerance or anything else alright any

questions about the basic framework yes

[Music]

you mean can you feed the output of the

reducers sort of oh yes oh yes in in in

in real life all right

in real life it is routine among

MapReduce users to you know define a

MapReduce job that took some inputs and

produce some outputs and then have a

second MapReduce job you know you're

doing some very complicated multistage

analysis or iterative algorithm like

PageRank for example which is the

algorithm Google uses to sort of

estimate how important or influential

different webpages are that's an

iterative algorithm is sort of gradually

converges on an answer and if you

implement in MapReduce which I think

they originally did you have to run the

MapReduce job multiple times and the

output of each one is sort of you know

list of webpages with an updated sort of

value or weight or importance for each

webpage so it was routine to take this

output and then use it as the input to

another MapReduce job oh yeah well yeah

you need to sort of set things up the

output you need to rate the reduced

function sort of in the knowledge that

oh I need to produce data that's in the

format or as the information required

for the next MapReduce job I mean this

actually brings up a little bit of a

shortcoming in the MapReduce framework

which is it's great if you are if the

algorithm you need to run is easily

expressible as a math followed by this

sort of shuffling of the data by key

followed by a reduce and that's it

my MapReduce is fantastic for algorithms

that can be cast in that form and we're

furthermore each of the maps has to be

completely independent and

are required to be functional pure

functional functions that just look at

their arguments and nothing else

you know that's like it's a restriction

and it turns out that many people want

to run much longer pipelines that

involve lots and lots of different kinds

of processing and with MapReduce you

have to sort of cobble that together

from multiple MapReduce distinct

MapReduce jobs and more advanced systems

which we will talk about later in the

course are much better at allowing you

to specify the complete pipeline of

computations and they'll do optimization

you know the framework realizes all the

stuff you have to do and organize much

more complicated efficiently optimize

much more complicated computations

from the programmers point of view it's

just about map and reduce from our point

of view it's going to be about the

worker processes and the worker servers

that that are they're part of MapReduce

framework that among many other things

call the map and reduce functions so

yeah from our point of view we care a

lot about how this is organized by the

surrounding framework this is sort of

the programmers view with all the

distributive stuff stripped out yes

sorry I gotta say it again oh you mean

where does the immediate data go okay so

there's two questions one is when you

call a MIT what happens to the data and

the other is where the functions run so

the actual answer is that first where

the stuff rotten there's a number of say

a thousand servers um actually the right

thing to look at here is figure one in

the paper sitting underneath this in the

real world there's some big collection

of servers and we'll call them maybe

worker servers or workers and there's

also a single master server that's

organizing the whole computation and

what's going on here is the master

server for know knows that there's some

number of input files you know five

thousand input files and it farms out in

vacations of map to the different

workers so it'll send a message to

worker seven saying please run you know

this map function on such-and-such an

input file and then the worker function

which is you know part of MapReduce and

knows all about Map Reduce well then

read the file read the input whatever

whichever input file and call this map

function with the file name value as its

arguments then that worker process will

employees what implements in it and

every time the map calls emit the worker

process will write this data to files on

the local disk so what happens to map

emits and is they produce files on the

map workers local discs that are

accumulating all the keys and values

produced by the maps run on that worker

so at the end of the math phase what

we're left with is all those worker

machines each of which has the output of

some of whatever maps were run on that

worker machine then the MapReduce

workers arrange to move the data to

where it's going to be needed for the

reduces so and since and a you know in a

typical big computation you know this

this reduce indication is going to need

all map output that

mentioned the key a but it's gonna turn

out you know this is a simple example

but probably in general every single map

indication will have produce lots of

keys including some instances of key a

so typically in order before we can even

run this reduce function the MapReduce

framework that is the MapReduce worker

running on one of our thousand servers

is going to have to go talk to every

single other of the thousand servers and

say look you know I'm gonna run the

reduce for key a please look at the

intermediate map output stored in your

disk and fish out all of the instances

of key a and send them over the network

to me so the reduce worker is going to

do that it's going to fetch from every

worker all of the instances of the key

that it's responsible for that the

master has told it to be responsible for

and once it's collected all of that data

then it can call reduce and the reduce

function itself calls reduce omit which

is different from the map in it and what

reduces emit does is writes the output

to a file in a cluster file service that

Google uses so here's something I

haven't mentioned I haven't mentioned

where the input lives and where the

output lives they're both files because

any piece of input we want the

flexibility to be able to read any piece

of input on any worker server that means

we need some kind of network file system

to store the input data and so indeed

the paper talks about this thing called

GFS or Google file system and GFS is a

cluster file system and BFS actually

runs on exactly the same set of workers

that work our servers that run MapReduce

and the input GFS just automatically

when you you know it's a file system you

can read in my files it just

automatically splits up any big file you

store on it across lots of servers and

64 megabyte chunks so if you write

if you view of ten terabytes of crawled

web page contents and you just write

them to GFS even as a single big file

GFS will automatically split that vast

amount of data up into 64 kilobyte

chunks distributed evenly over all of

the GFS servers which is to say all the

servers that Google has available and

that's fantastic that's just what we

need if we then want to run a MapReduce

job that takes the entire crawled web as

input the data is already stored in a

way that split up evenly across all the

servers and so that means that the map

workers you know we're gonna launch you

know if we have a thousand servers we're

gonna launch a thousand map workers each

reading one 1000s at the input data and

they're going to be able to read the

data in parallel from a thousand GFS

file servers thus getting now tremendous

total read throughput you know the read

through put up a thousand servers

so so are you thinking maybe that Google

has one set of physical machines among

GFS and a separate set of physical

machines that run MapReduce jobs okay

right so the question is what does this

arrow here actually involve and the

answer that actually it sort of changed

over the years as Google's

involve this system but you know what

this in those general case if we have

big files stored in some big Network

file system like you know it's like GFS

is a bit like AFS you might have used on

Athena where you go talk to some

collection and your data split over big

collection o servers you have to go talk

to those servers over the network to

retrieve your data in that case what

this arrow might represent is the meta

MapReduce worker process has to go off

and talk across the network to the

correct GFS server or maybe servers that

store it's part of the input and fetch

it over the network to the MapReduce

worker machine in order to pass the map

and that's certainly the most general

case and that was eventually how

MapReduce actually worked in the world

of this paper though and and if you did

that that's a lot of network

communication are you talking about ten

terabytes of data and we have moved 10

terabytes across their data center

network which you know data center

networks wanting gigabits per second but

it's still a lot of time to move tens of

terabytes of data in order to try to and

indeed in the world of this paper in

2004 the most constraining bottleneck in

their MapReduce system was Network

throughput because they were running on

a network if you sort of read as far as

the evaluation section their network

their network as was they had thousands

of machines

whatever and they would collect machines

they would plug machines and you know

each rack of machines and you know an

Ethernet switch for that rack or

something but then you know they all

need to talk to each other but there was

a route Ethernet switch that all of the

Rockies are net switches talked to and

this one and you know so if you just

pick some Map Reduce worker and some GFS

server you know chances are at least

half the time the communication between

them has to pass through this one

wouldn't switch their routes which had

only some amount of total throughput

which I forget you know some number of

gigabits per second and I forget the

number well but when I did the division

that is divided up to the total

throughput available in the routes which

by the roughly 2000 servers that they

used in the papers experiments what I

got was that each machine share of the

route switch or of the total network

capacity was only 50 megabits per second

per second in their setup 50 megabits

per second per machine and then might

seem like a lot 50 megabits gosh

millions and millions but it's actually

quite small compared to how fast a disks

Ron or CPUs run and so this with their

network this 50 megabits per second was

like a tremendous limit and so they

really stood on their heads in the

design described in the paper to avoid

using the network and they played a

bunch of tricks to avoid sending stuff

over the network when they possibly

could avoid it one of them was they

would they ran the gfs servers and the

MapReduce workers on the same set of

machines so they have a thousand

machines they'd run GFS they implement

their GFS service on that thousand

machines and run MapReduce on the same

thousand machines and then when the

master was splitting up the map work and

sort of farming it out to different

workers it would cleverly when it was

about to run the map that was going to

read from input file one it would figure

out from GFS which server actually holds

input file one on its local disk and it

would send the map for that input file

to the MapReduce software on the same

machine so that by default this arrow

was actually local local read from the

local disk and did not involve the

network and you know depending on

failures or load or whatever that

couldn't always do that but almost all

the maps would be run on the very same

machine and stored the data thus saving

them vast amount of time that they would

otherwise had to wait to move the input

data across the network the next trick

they played is that map as I mentioned

before stores this output on the local

disk of the machine that you run the map

on so again storing the output of the

map does not require network

communication he's not immediately

because the output stored in the disk

however we know for sure that one way or

another in order to group together all

of you know by the way the MapReduce is

defined in order to group together all

of the values associated with the given

key and pass them to a single invocation

to produce on some machine this is going

to require network communication we're

gonna you know we want to need to fetch

all bays and give them a single

machine that have to be moved across the

network and so this shuffle this

movement of the keys from is kind of

originally stored by row and on the same

machine that ran the map we need them

essentially to be stored on by column on

the machine that's going to be

responsible for reduce this

transformation of row storage

essentially column storage is called the

paper calls a shuffle and it really that

required moving every piece of data

across the network from the map that

produced it to the reduce that would

need it and now it's like the expensive

part of the MapReduce yeah

you're right you can imagine a different

definition in which you have a more kind

of streaming reduce I don't know I

haven't thought this through I don't

know why whether that would be feasible

or not certainly as far as programmer

interface like if the goal their

number-one goal really was to be able to

make it easy to program by people who

just had no idea of what was going on in

the system so it may be that you know

this speck this is really the way reduce

functions look and you know in C++ or

something like a streaming version of

this is now starting to look I don't

know how it look probably not this

symbol but you know maybe it could be

done that way and indeed many modern

systems people got a lot more

sophisticated with modern things that

are the successors the MapReduce and

they do indeed involve processing

streams of data often rather than this

very batch approach there is a batch

approach in the sense that we wait until

we get all the data and then we process

it so first of all that you then have to

have a notion of finite inputs right

modern systems often do indeed you

streams and and are able to take

advantage of some efficiencies do that

MapReduce okay so this is the point at

which this shuffle is where all the

network traffic happens this can

actually be a vast amount of data so if

you think about sort if you're sorting

the the output of the sort has the same

size as the input to the sort so that

means that if you're you know if your

input is 10 terabytes of data and you're

running a sort you're moving 10

terabytes of data across a network at

this point and your output will also be

10 terabytes and so this is quite a lot

of data and then indeed it is from any

MapReduce jobs although not all there's

some that significantly reduce the

amount of data at these stages somebody

mentioned Oh what if you want to feed

the output of reduce into another

MapReduce job and indeed that was often

what people wanted to do and

in case the output of the reduce might

be enormous like four sort or web and

mixing the output of the produces on ten

terabytes of input the output of the

reduces again gonna be ten terabytes so

the output of the reduce is also stored

on GFS and the system would you know

reduce would just produce these key

value pairs but the MapReduce framework

would gather them up and write them into

giant files on GFS and so there was

another round of network communication

required to get the output of each

reduce to the GFS server that needed to

store that reduce and because you might

think that they could have played the

same trick with the output of storing

the output on the GFS server that

happened to run the MapReduce worker

that ran the reduce and maybe they did

do that but because GFS as well as

splitting data for performance also

keeps two or three copies for fault

tolerance that means no matter what you

need to write one copy of the data

across a network to a different server

so there's a lot of network

communication here and a bunch here also

and I was this network communication

that really limited the throughput in

MapReduce

in 2004 in 2020 because this network

arrangement was such a limiting factor

for so many things people wanted to do

in datacenters modern data center

networks are a lot faster at the root

than this was and so you know one

typical data center network you might

see today actually has many root instead

of a single root switch that everything

has to go through you might have you

know many root switches and each rack

switch has a connection to each of these

sort of replicated root switches and the

traffic is split up among the root

switches so modern data center networks

have far more network throughput and

because of that actually modern I think

Google sort of stopped using MapReduce a

few years ago but before they stopped

using it the modern MapReduce actually

no longer tried to run the maps on the

same machine as the data stored on they

were happy to vote the data from

anywhere because they just assumed that

was extremely fast okay we're out of

time for MapReduce

we have a lab due at the end of next

week

in which you'll write your own somewhat

simplified MapReduce so have fun with

that

and see you on Thursday
