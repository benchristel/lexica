# The OGTRTA Instruction Set Architecture

A specification for a class of languages

## Determiners

- `DEF` singular definite article
- `DEFPL` plural definite article
- `NEGDET` negative determiner ("no man")

## Plural Markers

These are mostly used with indefinite nouns. When applied to definite nouns, they have a partitive meaning: "many of the inhabitants (plural)," "a few of the houses (paucal)," "all of the stars" (class plural)"

- `#PL` Plural
- `#PAU` Paucal
- `#CLS` Class Plural

## Prepositions

Derived from Wikipedia's [List of Grammatical Cases](https://en.wikipedia.org/wiki/List_of_grammatical_cases).

<details>

A language should have some way of expressing all of these ideas. Each preposition could probably have its own chapter in a grammar textbook.

- `at1` or `LOC1` "at" (the basic locative preposition) - "on/upon" can be merged with this.
- `of1` or `GEN1` "of" (genitive)
- `to1` or `ALL1` "to" (the basic allative preposition)
- `from1` or `ABL1` "from" (the basic ablative preposition)
- `in1` or `INESS1` "in" (the basic inessive preposition)
- `COM1` "with" (comitative)
- `INS1` "with" (instrumental)
- `AGT1` "by" (agentive) - can be merged with "with"
- `LIM1` "by" (limitative of time, e.g. "by 5:00")
- `DAT1` "for" (dative) - can be merged with "to"
- `BEN1` "for" (benefactive)
- `during1` "during"
- `DISTR1` "per" / "for each"
- `CMPR1` "like" / "as"
- `PRIV1` "without"

### Intransitive Prepositions

- `up0` &larr; `above1#MID`
- `down0` &larr; `below1#MID`
- `away0`, `off0` &larr; `from1#MID`

### Less Common Prepositions

- `between1` "between"
- `touch1` "touching"
- "about" / "concerning" - can be merged with "touching"
- `beside1` "next to" / "beside"
- `before1` "in front of" / "before"
- `behind1` "after" / "behind"
- `under1` "under"
- `over1` "over"
- `near1` "near"
- `ELA1` "out of" (elative)
- `PERL1` "through" / "via"
- "according to"
- "because of"
- "for want of"
- "instead of"
- `BEN1` "for the benefit of"
- `MAL1` "against" / "for the detriment of"

### Notes

Prepositions "to" and "from", and variants like "onto" and "from out of" can be replaced by inchoative and cessastive inflections of other prepositions. E.g. "to" can be `at1#INCH`. "for" (dative) can be `of1#INCH`.

</details>

## Personal Pronouns

- `1SG` `1PL`
- `2SG` `2PL`
- `3ANSG` `3ANPL` - animate 3rd person
- `3INANSG` `3INANPL` - inanimate 3rd person

## Demonstratives

- `here0`
- `there0`

## Atomic Verbs

- `say0`
- `go0`
- `know1`
- `see1`
- `like1` like (be fond of; the comparative preposition is `CMPR1`)
- `look1`
- `think1`
- `can1`
- `make1`
- `get1`
- `be1`
- `take`
- `find1`
- `ask2` - 1: askee, 2: thing requested
- `perceive2` - 1: thing perceived, 2: quality
- feel
- turn
- want
- stand
- die
- hear
- try
- open
- begin
- let `allow1`

## Derived Verbs

- `have1` &larr; `of1#PASS`
- tell &larr; `give2 <someone> know1#INF`
- `become1` &larr; `be1#INCH`
- give &larr; `of1#CAUS` or `have1#CAUS`
- last `endure0` &larr; `endure1#MID`
- seem, appear &larr; `perceive2#PASS`

## Negation

- `#NEG`

## Morphological Tenses

- `PROX` proximal tense, used for events in the present.
- `DIST` distal tense, used for the past and for hypotheticals.
- `AOR` aorist tense, used for general statements, for timeless truths, and when the speaker does not care to mark tense.

## Periphrastic Tenses

- `PRF` perfect (&larr; `ELA1` + infinitive)
- `FUT` future (&larr; `ALL1` + infinitive)

## Aspects

- `PROG` progressive (&larr; `LOC1` + infinitive)

## Valence-Changing

- `#MID` removes all complement slots.
- `#PASS` swaps the subject and first complement slots.
- `#MIDPASS` removes the subject and promotes the first complement to subject.
- `#CAUS` demotes the subject to first complement, increasing valence by 1, and adds a new subject.

## Valence Restoration

`#MID` removes all complement slots, but sometimes you don't want that; you want to keep one of the complements of a valence-2 predicate but remove the other. The way to accomplish that is via **valence restoration**.

Each predicate has a lexically-determined mapping from slot indices to prepositions that restore those slots. E.g. here `ABL1` is used to restore slot 1 of `ask2`.

- `1SG PROX FUT ask2#MID ABL1 3ANSG` "I'll ask them."

To restore slot 2, you'd have to use `ALL1`:

- `1SG PROX FUT ask2#MID ALL1 3INANSG` "I'll ask for it."

Valence restoration is also useful when you want to swap the complements of a valence-2 predicate:

- `1SG PROX FUT ask2#MID ALL1 3INANSG ABL1 3ANSG` "I'll ask for it from them."

## Part-of-Speech-Changing

### Nominalization

- `#INF` - nominalizes a predicate, keeping complement slots
- `#GER` - nominalizes a predicate, dropping all complement slots
- `NZ` - nominalizes a sentence

`GEN1`/`of1` can be used to attach a subject to a nominalized predicate:

- `1SG PROX want1 GEN1 2SG see1#INF 3INANSG` "I want you to see it"

### Questions

Interrogative sentences are NPs.

Interrogative pronouns:

- `what`
- `who`

Interrogative determiners:

- `which`
- `whose`

Interrogative particles:

- `whether`
- `how_condition` ("How are you?", "How is the course going?")
- `how_degree` (`how_degree AOR tall0 ^Barack ^Obama` "How tall is Barack Obama?" = "To what degree is Barack Obama tall?")
- `how_much`/`how_many`
- `why`

OGTRTA languages may front interrogative NPs, though some languages leave the pronoun in place. When a pronoun is fronted, a [resumptive pronoun](https://en.wikipedia.org/wiki/Resumptive_pronoun) `RES` is left in its place. Languages may realize the `RES` morpheme as [null](https://en.wikipedia.org/wiki/Null_morpheme).

- `whose vassal 3ANSG FIN PERF kill1#INF RES` "Whose vassal did he kill?"

## Agents in non-finite clauses

The semantic agents of a non-finite clause can be added as the complement of a modifier on the main predicate. This modifier is glossed `NFSBJ` (non-finite subject) but is typically realized as some other morpheme: either `of1` (in languages that [mark first modifiers](#first-modifier-marking)) or `ADV` (in languages with an [adverbial predicate](#adverbial-predicate)).

- `3ANSG PROX bring1 3INANSG` "He brings it." &rarr; `1SG PROX want1 NFSBJ 3ANSG bring1#INF 3INANSG` "I want him to bring it."

## Modifiers on predicates

Modifier phrases can attach to predicates as well as nouns. In complex sentences, this can create undesirable ambiguity. OGTRTA languages use a couple different strategies to resolve the ambiguity.

### Nearest modifier marking

In languages that use the "nearest modifier" strategy, a modifier that immediately follows the head of its parent phrase is marked with an inflection `#M`. Often, this marking is an initial consonant mutation, but it does not have to be.

- `1SG DIST eat0 quick0#M` "I ate quickly"
- `bear fierce0#M very0#M` "a very fierce bear" — contrast with `bear fierce0#M very0`, which might mean "an actual, fierce bear."

In SOV/OVS languages, a modifier that immediately _precedes_ the head of its parent phrase gets the `#M` inflection.

- `1SG quick0#M eat0 DIST` "I ate quickly"

### Adverbial predicate

In languages that use the "adverbial predicate" strategy, the only predicate that ever modifies another predicate directly is the one glossed `ADV1`. The complement of `ADV1` is typically a nominalized predicate:

- `1SG DIST eat0 ADV1 quick0#GER` "I ate quickly"
- `bear fierce0 ADV1 very0#GER` "a very fierce bear"

However, the complement of `ADV1` might also be a noun in some cases:

- `1SG DIST see1 3SG ADV1 thursday` "I saw it on Thursday"

`ADV1` may be realized as another lexeme, e.g. `LOC1`.