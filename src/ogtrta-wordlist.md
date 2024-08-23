# The OGTRTA Instruction Set Architecture

A specification for a class of languages.

Gloss abbreviations are drawn from Wikipedia's [list of glossing abbreviations](https://en.wikipedia.org/wiki/List_of_glossing_abbreviations).

OGTRTA is a template for making constructed languages. The idea is that with a short wordlist and a few decisions about grammar, you can create a fully-functioning, self-consistent, complete, and _unique_ conlang, suitable for further elaboration.

I created OGTRTA for my own use, because I suck at finishing conlang grammars. I wanted a way to create a conlang that was "complete" (if short on vocabulary) in a few minutes, so that most of the remaining work would be crafting the lexicon and tweaking the morphology. Essentially, I wanted the {{link terms/walking-skeleton}} pattern, but for conlangs instead of computer programs.

However, in order to arrive at a system that would work, the way I had to develop OGTRTA was to create several actual (half-assed) conlangs, peruse grammars of natural languages, and then figure out what framework could produce them all. OGTRTA is not some pie-in-the-sky idea I dreamed up. It's derived from observations of actual languages both natural and artificial.

OGTRTA comprises two parts: a syntax, and a set of glosses for about 50 morphemes. This document describes both parts, beginning with the syntax.

## Syntax

### Parts of speech

OGTRTA recognizes five parts of speech:

- nouns
- verbs
- determiners
- pronouns
- conjunctions

**Nouns** are straightforward: a noun refers to a person, place, thing, or idea. However, **verbs** in OGTRTA are a little bit different from English verbs. OGTRTA has no adjectives or prepositions, so verbs fill the role of both.

OGTRTA's **determiners** include words like articles (e.g. "the") and some interrogatives ("which", "whose").

**Pronouns** function syntactically much like nouns, but have some restrictions. For example, they cannot take determiners. Also, their inflectional morphology may differ from that of ordinary nouns (exactly _how_ it differs is left up to the individual language).

**Conjunctions** connect syntax nodes (phrases and sentences) as peers — i.e. without subordinating one node to the other. Conjunctions include such useful words as "and", "or", "while", "because", "so", and so on.

Of these parts of speech, nouns and verbs are "open classes," meaning that speakers of a language innovate and borrow new ones all the time. Determiners, pronouns, and conjunctions are "closed classes." Innovation in these parts of speech happens very rarely.

OGTRTA aims to provide languages with a complete set of determiners, pronouns, and conjunctions at the outset, so the only part of the lexicon you will need to design yourself is the nouns and verbs.

### Word order

The basic word order in OGTRTA is **VOS** or verb-object-subject. Wait, come back! The subject is usually fronted via a transformation, so **SVO** word order is more common in actual sentences. Additionally, OGTRTA is a reversible syntax: the right-hand side of all the syntax production rules can be reversed, producing an **SOV** language. Backing the subject of an SOV sentence produces **OVS**.

The other possible word orders, **VSO** and **OSV**, are not directly supported. If you really want to make a VSO language with OGTRTA, you can probably figure something out, but this guide will not describe how to do it.

To keep things straightforward, this guide assumes a verb-object word order, and all the examples will use that syntax.

### Production rules, level 1

The complete set of production rules for OGTRTA is complex, so rather than dump them on you all at once, they will be introduced in stages. This section describes "level 1" syntax—the most minimal, stripped-down version of the grammar.

A **sentence** `S` consists of a **verb phrase** `VP` and a subject **noun phrase** `NP`.

```
S -> VP NP
```

The order of nodes in a sentence can also be reversed:

```
S -> NP VP
```

A sentence can also be composed of multiple sentences, using a conjunction:

```
S -> S CONJ S
```

A **verb phrase** consists of a **verb** `V` with valence `n` (represented `V/n`), followed by n **complement noun phrases** (represented `NP{n}`).

```
VP -> V/n NP{n}
```

What's **valence**, you ask? The valence of a verb is the number of noun phrases you need to put after the verb to make it a complete "thought." For example, "Rachel skydives" is a complete thought, meaning "skydives" has a valence of 0, but "Rachel pokes" is not complete — "pokes" has valence 1 and requires an object.

Note that _valence is a grammatical concept, not a logical one_. In English, a verb like "eat" can have either valence 1 or valence 0, and that's perfectly fine. You can say "I eat", or "I eat beans" and both are grammatical. Your language might work similarly. In any case, valence is always about what _words_ are required to be there, not about whether the action represented by the verb conceptually has a direct object or not. This clean separation between semantics and syntax is what enables OGTRTA to work.

A **noun phrase** consists of a noun, optionally preceded by a determiner:

```
NP -> DET? N
```

A noun phrase can also be composed of other noun phrases, using a conjunction:

```
NP -> NP CONJ NP
```

Or it can just be a pronoun:

```
NP -> PRN
```

With just these syntax rules, we can already construct sentences of arbitrary length and complexity. However, you probably sense that something's missing. Where are the adjectives?

### Production rules, level 2

Recall that OGTRTA does not have adjectives as a separate lexical class. However, its syntax does have a concept of **modifiers**. A verb phrase can modify a noun by following it:

```
NP -> DET? N VP
```

In fact, a noun can be modified by any number of VPs (zero or more), represented by `VP*`:

```
NP -> DET? N VP*
```

Words that we think of as adjectives in English are zero-valence verbs in OGTRTA. 

A noun phrase that includes a determiner can have zero-valence modifiers between the determiner and the noun:

```
NP -> DET V_0* N VP*
```

This rule is optional, though, and individual languages can safely leave it out.

Verbs can also have modifiers. The syntax is slightly more complex because of the presence of complements:

```
VP -> V_n VP* NP{n} VP*
```

That is, a verb's modifiers should go before all the complements, or after all the complements. Putting them between the complements would be confusing! (Actually, this isn't a hard and fast rule; your language can certainly break it. But writing down that more general rule in formal language would be hard, and I feel like conlanging is hard enough already.)

### Production rules, level 3

Now we come to the interface between syntax and morphology. The nodes in our syntax tree can have "tags," which can be "inherited" by descendent nodes. Depending on the specific language, tags on a node can influence what morphological affixes are allowed or required. In the syntax metalanguage, I'll represent these tags as a letter after a dot, like `.x`.

So, starting back at the beginning: A sentence has a **subject noun phrase** (`NP.s`) and a **finite verb phrase** (`VP.f`).

```
S -> VP.f NP.s
S -> NP.s VP.f
```

Any tags on a verb phrase are inherited by its head verb. Here I use `.x` to mean "any tags that are present." The modifiers of a phrase get a "modifier tag" `.m`, and the complements of a verb may get a "case tag" `.c`, the specific nature of which is determined by the lexical verb. The modifiers of a verb phrase also get an "adverbial tag" `.a`.

```
VP.x -> V_n.x VP.m.a* NP.c{n} VP.m.a*
```

That rule is pretty complicated, but I promise it's the worst one. A conjoined verb phrase passes its tags to both conjuncts:

```
VP.x -> VP.x CONJ VP.x
```

Same with a conjoined noun phrase:

```
NP.x -> NP.x CONJ NP.x
```

The head noun of an NP inherits the tags:

```
NP.x -> PRN.x
NP.x -> DET? N.x VP.m*
NP.x -> DET V_0.m* N.x VP.m*
```

And that's it! That's the entire syntax of OGTRTA.

All together now:

```
S -> VP.f NP.s
S -> NP.s VP.f
S -> S CONJ S
VP.x -> V_n.x VP.m.a* NP.c{n} VP.m.a*
VP.x -> VP.x CONJ VP.x
NP.x -> PRN.x
NP.x -> DET? N.x VP.m*
NP.x -> DET V_0.m* N.x VP.m*
NP.x -> NP.x CONJ NP.x
```

Get that printed on a t-shirt. I'm sure it'll be a hit at parties.

### Production rules appendix: agreement tags

As described above, the syntax of OGTRTA can do many things, but it has one major shortcoming: it cannot represent any constraints on _agreement_ between words. That is, the syntax cannot require a verb to agree in plurality with its subject (as in English "he swims" vs. "they swim"), or a modifier to agree in gender with the noun it's modifying (as in Spanish "el poema bonito" vs. "la casa bonita").

All is not lost, however: we can use tags to represent agreement constraints between nodes. It's not possible to fully specify a formal system for doing this, because it's very dependent on the individual language, but I'll give some examples.

First example: perhaps you want your language to have masculine and feminine articles, like Spanish does, and you want to require that nouns must agree in gender with their article. Here's how you might do that with `.m` and `.f` tags:

```
NP.m -> DET.m N.m VP*
NP.f -> DET.f N.f VP*
```

These rules say, basically, that a noun phrase can be either masculine or feminine. Masculine NPs have to have a masculine determiner and a masculine noun, while feminine NPs have to have a feminine determiner and a feminine noun.

Maybe you also want to head-mark the gender of VP constituents on the verb, as in Swahili. Here's how you'd do that, with tags `ms` and `fs` for the gender of the subject and `m1` and `f1` for the gender of the first complement.

```
S.ms.m1 -> VP.ms.m1 NP.ms
S.ms.f1 -> VP.ms.f1 NP.ms
S.fs.m1 -> VP.fs.m1 NP.fs
S.fs.f1 -> VP.fs.f1 NP.fs
VP.ms.m1 -> V_1.ms.m1 VP* NP.m1 VP*
VP.ms.f1 -> V_1.ms.f1 VP* NP.f1 VP*
VP.fs.m1 -> V_1.fs.m1 VP* NP.m1 VP*
VP.fs.f1 -> V_1.fs.f1 VP* NP.f1 VP*
```

As you can see, this gets more complicated the more genders and complements you have to deal with. But in theory, this system is capable of expressing all the possible combinations.

This is a sketch, not a formal description. It's hard to describe agreement tags precisely in any system less powerful than an actual programming language. But hopefully you get the idea. If not, don't worry about it. You don't need to know anything about agreement tags unless you, like me, need the security blanket of formality to keep those evil, messy WORDS safely away from you.

## Determiners

### Articles

- `DEF` singular definite article
- `DEFPL` plural definite article

For indefinite nouns, number is expressed periphrastically, with modifiers.

This difference in number marking strategies stems from an observation about
how number is used in practice. In many indefinite noun phrases, the noun is
effectively numberless:

- "Are there **sheep** on the hill?"
- "Everyone should send their **children** to school."

These examples are morphologically plural (in English) but the actual
referents of the highlighted nouns might be singular or plural depending
on the circumstances of the utterance.

However, a definite noun cannot be numberless, because a definite noun
always refers to a specific set of things.

### Quantifiers

- `NEGDET` negative quantifier ("there are **no sheep** on the hill")
- `every` universal quantifier ("**every sheep** is on the hill")

### Interrogatives

- `which`
- `what_kind_of`
- `how_much` (also used for "how many")

### Words that aren't determiners

Some types of words are determiners in English but not in OGTRTA. These include:

- possessive pronouns ("my", "your")
- demonstratives ("this (one)", "that (one)")

These concepts are all expressed by **verbs** in OGTRTA.

### Semantics

OGTRTA employs determiners only in cases where the referent of a noun cannot be pinned down by [determining modifiers](#TODO). A phrase like "the bear" cannot be paraphrased by describing a _kind_ of bear — the same goes for "no bear" and "every bear".

Possessives and demonstratives, on the other hand, _can_ be expressed as modifiers. Precedents exist in many natural languages: English ("for the sake **of them**"), Italian ("_la **mia** famiglia_"), and Welsh ("_y bore **'ma**_"), for instance. Note that in each of these cases, the modifier can coexist with the definite article.

### "every" / "all" / "none" as modifiers

Since determiners cannot be used with pronouns, a periphrastic construction is needed to express e.g. "all of them".

- `1SG DIST see1 every being of1#M 3ANPL` "I saw all of them" (lit: I saw every one of them)

`NEGDET` uses a different paraphrase:

- `1SG DIST see1 NEGDET being of1#M 3ANPL` "I saw none of them"

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

`#MID` removes all complement slots, but sometimes you don't want that; you want to keep one of the complements of a valence-2 verb but remove the other. The way to accomplish that is via **valence restoration**.

Each verb has a lexically-determined mapping from slot indices to verbs that restore those slots when used as modifiers. E.g. here `ABL1` is used to restore slot 1 of `ask2`.

- `1SG PROX FUT ask2#MID ABL1#M 3ANSG` "I'll ask them."

To restore slot 2, you'd have to use `ALL1`:

- `1SG PROX FUT ask2#MID ALL1#M 3INANSG` "I'll ask for it."

Valence restoration is also useful when you want to swap the complements of a valence-2 verb:

- `1SG PROX FUT ask2#MID ALL1#M 3INANSG ABL1 3ANSG` "I'll ask for it from them."

Modifiers used for valence restoration never take the [adverbial particle](#adverbial-particle).

## Part-of-Speech-Changing

### Nominalization

- `#INF` - nominalizes a verb, keeping complement slots
- `#GER` - nominalizes a verb, dropping all complement slots
- `NZ` - nominalizes a sentence

`GEN1`/`of1` can be used to attach a subject to a nominalized verb:

- `1SG PROX want1 GEN1#M 2SG see1#INF 3INANSG` "I want you to see it"

When used in this way, `GEN1` never takes the [adverbial particle](#adverbial-particle).

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
  - also used for `how_much`/`how_many`
- `why`

An _interrogative NP_ is either an interrogative pronoun, or a noun with an interrogative determiner.

TODO: replace interrogative determiners with predicates. Possessives and demonstratives aren't determiners, so why should `which` and `whose` be?

OGTRTA languages may front interrogative NPs, though some languages leave them in place. When an interrogative NP is fronted, a [resumptive pronoun](https://en.wikipedia.org/wiki/Resumptive_pronoun) `RES` is left in its place. Languages may realize the `RES` morpheme as [null](https://en.wikipedia.org/wiki/Null_morpheme).

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

### Adverbial particle

In languages that use the "adverbial particle" strategy, a verb that modifies another optionally has the particle `ADV` placed before it.

- `1SG DIST eat0 ADV quick0` "I ate quickly"
- `bear fierce0 ADV very0` "a very fierce bear"

`ADV` might also precede a noun in some cases:

- `1SG DIST see1 3SG ADV thursday` "I saw it on Thursday"
