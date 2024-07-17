# The OGTRTA Wordlist of Unspecified Canonicity

A checklist for my languages

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
- `during1` "during"
- `DISTR1` "per" / "for each"
- `CMPR1` "like" / "as"
- `PRIV1` "without"

### Intransitive Prepositions

- `up0`
- `down0`

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
- "for the benefit of"
- "against" / "for the detriment of"

### Notes

Prepositions "to" and "from", and variants like "onto" and "from out of" can be replaced by inchoative and cessastive inflections of other prepositions. E.g. "to" can be `at1#INCH`. "for" (dative) can be `of1#INCH`.

</details>

## Personal Pronouns

- `1SG` `1PL`
- `2SG` `2PL`
- `3SG` `3PL`

## Demonstratives

- `here0`
- `there0`

## Common Verbs

- have &larr; `of1#PASS`
- say
- go
- know
- see
- `like1` like (be fond of; the comparative preposition is `CMPR1`)
- look
- think
- can
- make
- get
- take
- find
- give
- ask
- feel
- seem

## Morphological Tenses

- `PROX` proximal tense, used for events in the present.
- `DIST` distal tense, used for the past and for hypotheticals.
- `AOR` aorist tense, used for general statements, for timeless truths, and when the speaker does not care to mark tense.

## Periphrastic Tenses

- perfect (using `ELA1` + infinitive)
- future (using `ALL1` + infinitive)
