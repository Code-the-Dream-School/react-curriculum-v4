# Discussion Notes

## Week 6 - Re-usable Components, Project Organization, Refactoring, and Testing

SarahSoutoul on Apr 16 Collaborator

Everything about re-usable components and project org is really important to keep in my opinion. The testing part is the one l'm
unsure about - few suggestions:

• Incorporate testing principles in Intro to Programming course (if it's not there already), so we can jump straight into how testing
is done in React from a practical stand point and reduce the theory in the notes
• Using the same method as the first point but incorporate testing across existing materials instead of one only section around it
• Remove it completely on the basis that they could have that as supplemental learning / extra session potentially

rebekahcallkacz on Apr 17

I love the idea of introducing testing earlier. As a React mentor, I usually started with writing tests for helper functions because
it was so unfamiliar to students and then l'd do a follow up on how to write it for React components.

I'm also all for incorporating testing across existing materials - I'd probably start in week 2 or 3. And, l'd probably add setup to
Week 1 if we went that route.

## Week 7 - Data fetching, Conditional Rendering, UI Update Strategies

For this one, refreshing students knowledge on data fetching might be useful but I would keep that section shorter (not even mentioning the Then/Finally/Catch stuff as we would expect them to use async/await anyway). Potentially, we could have a line that says "if you want to revisit fetching and async/await, head to this link type of thing".

1 reply 1 new
@rebekahcallkacz
rebekahcallkacz
on Apr 17
Yeah, I like the idea of the review materials being in a separate link/page. We'll just need to follow up with the Intro course to make sure it stays in their curriculum.

royemosby
on Apr 17
Maintainer
Author
I suggest combining conditional rendering with JSX - the topic reduces down to using some condition to show/hide an element. That can be used as examples to increase the student's exposure to js in xml.

1 reply
@rebekahcallkacz
rebekahcallkacz
on Apr 17
I like this idea if we can figure out the logistics of doing it in a way that makes sense in the project/UI at that time when they aren't using useState or fetching data.

## Week 4 - Basic Hooks, Events and Handlers, Updating State

SarahSoutoul
on Apr 16
Collaborator
Few things here:

Previous to this, there is a whole lesson on state and useState, so I'm not sure about having a whole section of that again in this week's lesson. Def good to refresh students knowledge, but that section could be shorter in that regard.
We might need to remove some of the hooks mentioned in the notes like useMemo or useCallback if we remove them entirely from the curriculum
0 replies

royemosby
on Apr 17
Maintainer
Author
State and useState can be consolidated into 1 lesson. Hooks should emphasize more use of dev docs after working through useEffect, and useRef. Hooks should probably come before useState since it's a hook.

This was one of the areas I was having a hard time decoupling a few concepts for discussion. Which comes first, how to we present it, and how would we update the assignments?

0 replies

rebekahcallkacz
on Apr 17
+1 to removing useCallback and/or useMemo or moving them to stretch goals.

I do think useState makes sense to be the first hook that we teach since it's the easier one IMO. Maybe the useState intro has a brief introduction to what hooks are and then the second lesson with useRef and useEffect goes into more detail. Hooks are pretty essential and oftentimes pretty unfamiliar too so I think some repetition is a good thing.

1 reply 1 new
@SarahSoutoul
SarahSoutoul
on Apr 17
Collaborator
Completely agree with this! Think there should be a lesson introducing hooks with useState as the first one. And then, continue with other hooks in another lesson. That's usually the order I use when teaching React.

## Week 12 - Pagination and React-Router

royemosby
on Apr 17
Maintainer
Author
We could consolidate non-react specific UI concepts into a "cookbook" or supplement.

0 replies

rebekahcallkacz
on Apr 17
This is another one that could be condensed into another week if need be.

0 replies

linh-pham19
on Apr 17
Collaborator
Pagination could be stretch goal for anyone interested in learning more about it. I think React-router should be kept as it's react related
s
