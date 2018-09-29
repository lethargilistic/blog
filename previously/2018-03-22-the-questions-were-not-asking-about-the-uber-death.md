# The Questions We're Not Asking About the Uber Death 
(2018-03-22, [dev.to](https://dev.to/lethargilistic/the-questions-were-not-asking-about-the-uber-death-fj5))

*It came out later that the Uber developers deactivated the car's safety features like automatic stopping while the automated driver was engaged. That level of negligence leading to the death was murder. Full stop. Uber's developers murdered a woman, and they should be held personally accountable in addition to the corporation's receiving financial penalties. Our rush to automated vehicles should not be killing bystanders without penalties*

*"But cars driven by humans kill people all the time." Yeah. And they should be held accountable if they negligently kill pedestrians, but we know that they generally aren't. Creating an automated system and shifting the goalpost blame for pedestrian deaths is extremely counterproductive to solving the human problem of assigning blame after a car accident&mdash;you ever notice that we immediately call it a car "accident," even before we figure out whether or not the cause of the crash was accidental?*

-----

You've probably already heard about the death in Arizona. Some people are questioning whether or not the technology is safe. I'm not here to talk about them, so I'll dismiss those fears right off the bat. The technology is in its infancy today and you can use your imagination to picture where this will go in the long run. The future of automobiles is autonomous, and they will be safe by the time your descendants are calling what we drive today "dumb cars" like we call feature phones "dumb phones."

This post addresses a different kind of reactionary: the technologists rushing to defend this individual autonomous vehicle because of the great potential of the technology used to create it. Many of these people are engineers, and among them are people for whom I feel a great deal of respect.  These people are not thinking like engineers. They are thinking like people afraid that their new toy *might* be taken away from them. They jump past specific details of the crash, technical details that they would consider if they thought about this as a software problem, and foundational questions we would ask of *any* human driver who had hit a pedestrian.  

If you have not asked any of these questions, you are not qualified to have an opinion on whether or not Uber should have any culpability in the woman's unfortunate death.  

## What was the car's speed?
Do you know? Did you want to know before I asked? I hope so, but bear with me.

If you had a case of a human driver hitting a pedestrian, the first thing you'd be doing would be trying to think of ways that the driver may have been negligent. The first and almost only thing we've been trying to do is think of ways to exonerate the car. Extremely often, cases of death via vehicle are subject to a very strong "survivor bias" because only the human who lived has lived to tell the tale about how there was nothing to be done.

But we have video! That's cool. It shows us visual details about the time immediately before the death. It shows us no other details, qualitative or quantitative about the state of the car or the area. How fast was the car moving? Was the car correcting for any deficiency of the road? Were the lights as bright as they could have been? Was there a car to the left? Was there any shoulder room on the right?

Why are we grading this tool meant to serve us with a rubric much easier than we would have used if the dead human had been behind the wheel? These cars are supposed to be better drivers than us, right? Why would the idea that we, personally, could not have prevented the death with one of our senses mean that the machine with its array of visual and non-visual mechanical sensors couldn't have done better in this case?

The car was going 40 miles per hour, incidentally. That was included in news reports later. Did you know?

## Was the car going the speed limit?
Related to the above for obvious reasons, but worthy of its own scrutiny. We assume the answer is "yes" because we assume these things are infallible because one day they *will be* unfallible. This is a fallacy. It will still be a fallacy when autonomous vehicles are the majority of cars on the road. We have not solved this problem; we are in the infancy of the process of solving it.

So, why are we assuming that it was going the speed limit because all autonomous vehicles in the future will go the speed limit?

Why are we assuming that the cameras were perfect and would have recognized a speed limit sign in these conditions? If the camera had trouble seeing her in that dark in the middle of the road, how can we possibly be sure that it saw a speed limit sign on the side of the road?

Maybe the car doesn't even rely on signs for this. Why are we assuming today's method is perfect?  We shouldn't, especially if it's not entirely public.

## What would have happened if the driver had not been on their phone?
I want to say straight out that I am ***NOT*** trying to make it seem like the person behind the wheel needs to feel responsible. But, if they had been paying attention, and assuming that they saw the woman when the camera did not, they would have hit the breaks. Would that have made the difference? They will live with that question just like people who were actually in control of their cars have to.  

We're already fantasizing about being in cars without steering wheels and being able to have that extra cup of coffee in the morning. It's a selling point. The advent of the autonomous car will also be the normalization of people not paying attention to the road. In this transitional phase, does that mean that accidents that could be prevented will be made to happen? Are we to just accept that loss of life as inevitable, shrugging it off and saying the pedestrian deserved a Darwin Award?

That is unacceptable. If one death in this phase could be prevented by better preserving the driver's attention behind the wheel, that is an avenue of research worth pursuing during this transition.  

## The camera recorded her. Did the car detect her?
Let's turn this over to our engineering friends in the audience. Imagine you're walking into your standup meeting one morning and a senior developer announces that the company's flagship software refuses to load. You have the following exchange:

    "What investigations have we performed?"

    "I saw an error message on the terminal."

    "...I'd imagine so. What did the debugger say?"

    "I don't have any debugger output."

This would be unacceptable. The flagship is down and you haven't even begun to share details critical to determining the issue, let alone solving it? Now imagine if other developers in the room nodded their heads at this and determined the system had functioned properly and there was nothing that could have been done to prevent this. Madness.

At a certain point in the video, your human eyes see a form in front of the car and recognizes it as a person, more generally an obstacle. The computer inside the car works a lot faster than you, trying to do the same thing. My question is, when did the car recognize this obstacle? Did it ever? Where is our copy of the video with debugging information about where the road is and what's in front of the car?  

At this stage, we cannot discern what the car was thinking when it hit her, and therefore we *cannot* completely exonerate the car. Has Uber even disclosed this debugging information to the police? We have to know what the car thought about its readings if we're to make a decision about liability. I've been reading a lot of the stories about this, and it hasn't even been explicitly reported whether or not the car *stopped without human intervention*. I *assume* so. I *hope* so. I *do not* ***know*** so.

If this footage shows that the car never recognized the person, Uber should absolutely be liable for damages in this situation. If, instead of a person, it had been some object owned by another, we would not accept "I didn't see it" as a reason for declaring no fault. Aside, I would go further and say that if that debugging video and the raw data to replicate it is not made public, then Uber should be punitively liable for full damages. It's that important. We can't set the precedent here that it's OK to judge these machines by video alone when we can have access to all their readings.

## Was the Accident Similar to Any Previous Test Cases?
Jumping to blame the pedestrian, refusing to properly analyze these events, and saying "that's that" also has a terrible second order effect. Try to forget about the death for a minute. Forget about all the automated vehicle accidents we've faulted human error for. What if only looking for catastrophic failures is blinding us to mistakes that have yet to result in accidents? What are the boring mistakes, like where they take a turn the wrong way or misidentify something and adjust trajectory improperly? If there are no issues like this and the software is perfect (no software is perfect), then what are the developers working on? If it's perfect (it isn't), ship it worldwide tomorrow.  

We cannot know how safe these vehicles are without their testing data being made
public. 

I've harped on us assuming that they are safe because they will one day be perfectly safe enough, but why do we think they are safe today without any actual data? We are told that they have a good track record on the road and that most crashes involving them have been the fault of dumb cars and their drivers.  We are told that the system behind their driving has been tested on billions of miles of virtual road and on specially designed courses. All of that can be true, but we cannot trust these vehicles if we cannot independently verify that they are safe.

Call on these companies to release data on this. If they don't, we need to get regulatory agencies involved. People's lives are at stake.  

## Concluding remarks
Maybe I come off as too fixated on the camera footage, but the video makes this easier to visualize and it's what we've been given so far. Uber does use non-visual sensors as well. Why didn't those detect a woman directly in front of the vehicle? Is their range insufficient for a reaction to a sudden obstacle when the vehicle is traveling 40 miles per hour? That's an important safety question if this is to become the new normal.  

I started this list by saying that you were not qualified to have an opinion if you did not ask these questions. I'm sure there were more questions, but now you're informed about these. Now, I want you to think back to those first few hours after reports of Elaine Herzberg's death and remember forever if you jumped, for whatever reason, to the assumption that she caused her own death.

You did not have the information to answer all of these questions&mdash;we still don't. You did have the capacity to begin *asking* all of these questions. I was quite disappointed by the people who decided to turn a blind eye and cold heart toward loss of human life in the name of their idealized futurism. We will not create that future if we are not critical of the technology that we're creating.
