@def title = "To Perceive Is To Value"  
@def tags = ["rl", "philosophy", "meta", "arguments", "misc", "essay"]  
@def isblog = true  
@def showall = true 
@def rss_pubdate = Date(2026, 6, 26)  
@def rss_title = "To Perceive Is To Value"  
@def rss_description = "On state abstraction, value hierarchies, and what any observer requires"


# To Perceive Is To Value: On state abstraction, value hierarchies, and what any observer requires

Any agent operating in a world it cannot fully observe must make a choice: what to keep.
This is the state abstraction problem, and it sits at the foundation of reinforcement learning.
The standard framing treats it as a representational question: find an abstraction that preserves value-equivalence, bisimulation distance, or affordance structure.
What is rarely asked is what has to already be true about the agent for any of these criteria to make sense.
The answer, I want to argue, has consequences that reach well beyond representation theory.

The universe, or at least the part any particular agent inhabits, contains more information than any bounded system can process.
To perceive at all is therefore to project: to map a high-dimensional state onto something tractable. 
This is not a limitation unique to biological agents or current neural architectures.
It is structural.
An agent that processed all available information without compression would have no basis for action, because action requires differentiation: some states must matter more than others. The moment you have differential salience, you have a value structure.
Perception is not prior to valuation. It is constituted by it.

This has a precise technical shadow.
Abel's work on state abstraction shows that the quality of an abstraction depends entirely on what you are trying to preserve.
Value-irrelevance abstractions group states whse differences do not affect optimal policy under a fixed reward.
Bisimulation metrics measure structural similarity in terms of reward and transition dynamics.
Harutyunyan's work on successor representations shows that what an agent predicts, the structure of its anticipations, shapes what it can transfer and generalize.
In each case, the abstraction is downstream of a commitment: to a reward function, to a set of predictions, to a goal.
You cannot choose a representation without already having, implicitly, something you care about.
The representation encodes the caring.

Jordan Peterson, in one of his discussions with Alex O'Connor, makes a structurally identical argument outside RL.
When you perceive a glass of water, you do not see a neutral physical object and then assign meaning to it.
You see a container, whch presupposes thirst, which presupposes the value of bodily continuity, wihch presupposes survival, which presupposes something beyond survival that makes survival worth pursuing.
Trace this chain far enough and you arrive at questions that cannot themselves be grounded in anything further. 
Peterson claims this foundation God: not a being in the world, but the ungrounded base of the value hierarchy that makes any paticular valuation coherent.
This is not a theological claim in the conventional sense
It is a structural one.
Any value hierarchy, if it is coherent, must bottom out somewhere, and whatever it bottoms out on cannot itself be justified by the hierarchy without circularity.
The foundation precedes the compression.
It is what makes the compression possible at all.

Krishnamurti arrives at the same place from another direction.
He argues that the observer is not separate from the observed: what you see is inseperable from how you are constituted to see it.
In the compression framing, this is precise.
The abstraction does not sit betweena  pre-existing self and a pre-existing world.
The self is the abstraction.
The "I" that perceives is the particular compression scheme instantiated in a body with a history.
Observer and observed co-arise through the same operation.
Language makes this hard to hold because its subject-verb-object structure forces a grammatical separation onto what is, in immediate experience, a unified event.
When you say "I see the tree," language has already performed the division that Krishnamurti, and before him Heidegger, were trying to undo.
The word is not the thing, but more troubling: the word creates the gap between the one who names and the thing named.

For continual learning, this reframing has real stakes.
Plasticity loss is not only an optimization failure.
If the agent's compression scheme becomes rigid, unable to reorganize around new structure as tasks shift, then what has failed is the agent's capacity to reconstitute itself.
The value hierarcy has calcified.
Catastrophic forgetting, in this light, is what happens when a prior self prevents a new one from forming.
The standard fixes, replay, regularization, architectural modularity, are engineering responses to a problem that is, at its foundation, about what it means for an agent to remain genuinely open to new ways of mattering.

Several questions follow that I do not know how to answer. 
If any compression scheme encodes a value structure, what does it mean for two agents to share a representation?
Are they implicitly sharing a proto-value system and what are the implications for multi-agent continual learning?
Abel's bisimulation abstractions are grounded in a fixed reward, but in continual settings the reward drifts: what is the right notion of abstraction when th thing organizing the compression is itself non-stationary?
And the deepest question: is there a compression scheme that is maximally general, one that does not privilege any particular value structure?
If so, does it correspond to anything coherent, or does maximal generality collapse into no perception at all?
A truly neutral agent may be incoherent as a concept.
Every representation is a bet on what matters.
In continual learning, the bet must be revised.
The question is whether you can revise it without becoming a different agent entirely, and whether, in a sufficiently non-stationary world, that distinction is even meaningful.