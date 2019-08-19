# Zero Knowledge Proof

## From Wikipedia

Based on https://en.wikipedia.org/wiki/Zero-knowledge_proof

---

In cryptography, a **zero-knowledge proof** or **zero-knowledge protocol** is a method by which one party (the prover) can prove to another party (the verifier) that they know a value *x*, without conveying any information apart from the fact that they know the value *x*.

> zero-knowledge proof는 prover가 verifier에게 x라는 값을 x 전달 없이 알고있다고 증명 하는 프로토콜

The essence of zero-knowledge proofs is that it is trivial to prove that one possesses knowledge of certain information by simply revealing it; ***the challenge is to prove such possession without revealing the information itself or any additional information.***

If proving a statement requires that the prover possess some secret information,

- then the verifier will not be able to prove the statement to anyone else without possessing the secret information.
- The statement being proved must include the assertion that ***the prover has such knowledge, but not the knowledge itself.***

Otherwise, the statement would not be proved in zero-knowledge because it provides the verifier with additional information about the statement by the end of the protocol.

- A *zero-knowledge proof of knowledge* is a special case when the statement consists *only* of the fact that the prover possesses the secret information.

***A protocol implementing zero-knowledge proofs of knowledge must necessarily require interactive input from the verifier.***

- This  interactive input is usually in the form of one or more challenges such that the responses from the prover will convince the verifier if and  only if the statement is true, i.e., if the prover *does* possess the claimed knowledge.

If this were not the case, the verifier could record the execution of the protocol and replay it to convince someone else that they possess the secret information. The new party's acceptance is either justified since the replayer *does* possess the information (which implies  that the protocol leaked information, and thus, is not proved in  zero-knowledge), or the acceptance is spurious(거짓된), ***i.e., was accepted from someone who does not actually possess the information.***

### Abstract examples

#### The Ali Baba cave

It is common practice to label the two parties in a zero-knowledge proof as Peggy (the **prover** of the statement) and Victor (the **verifier** of the statement).

> Peggy(prover) <-> Victor(verifier)

Victor wants to know whether Peggy knows the secret word; but Peggy, being a very private person, does not want to reveal her knowledge (the secret word) to Victor or to reveal the fact of her knowledge to the world in general.

They label the left and right paths from the entrance A and B. First, Victor waits outside the cave as Peggy goes in. Peggy takes either path A or B; Victor is not allowed to see which path she takes. Then, Victor enters the cave and shouts the name of the path he wants her to use to return, either A or B, chosen at random. Providing she really does know the magic word, this is easy: she opens the door, if necessary, and returns along the desired path.

However, suppose she did not know the word. Then, she would only be able to return by the named path if Victor were to give the name of the same path by which she had entered. Since Victor would choose A or B at random, she would have a 50% chance of guessing correctly. ***If they were to repeat this trick many times, say 20 times in a row, her chance of successfully anticipating all of Victor's requests would become vanishingly small (about one in a million).***

> 만약 Peggy가 옳바른 word 모르고 있으면 여러번 반복 할 수록 Peggy가 거짓말 성공 할 수 있는 확률 낮아짐
>
> ex) A로 나와라 -> B로 나와라 -> ... -> A로 나와라  : 모두 맞추기 힘듬

One side note with respect to third-party observers: even if Victor is wearing a hidden camera that records the whole transaction, the only thing the camera will record is in one case Victor shouting "A!" and Peggy appearing at A or in the other case Victor shouting "B!" and Peggy appearing at B.

***A recording of this type would be trivial for any two people to fake (requiring only that Peggy and Victor agree beforehand on the sequence of A's and B's that Victor will shout).*** Such a recording will certainly never be convincing to anyone but the original participants. In fact, even a person who was present as an observer *at the original experiment* would be unconvinced, ***since Victor and Peggy might have orchestrated the whole "experiment" from start to finish.***

> Prover와 Verifier가 미리 짜고 Observer를 속일 수 있음 -> 처음부터 참가하던 사람만 납듭 가능

***Further notice that if Victor chooses his A's and B's by flipping a coin on-camera, this protocol loses its zero-knowledge property;*** the on-camera coin flip would probably be convincing to any person watching the recording later. Thus, although this does not reveal the secret word to Victor, it does make it possible for Victor to convince the world in  general that Peggy has that knowledge—counter to Peggy's stated wishes. ***However, digital cryptography generally "flips coins" by relying on a pseudo-random number generator, which is akin(~와 유사한) to a coin with a fixed pattern of heads and tails known only to the coin's owner.*** If Victor's coin behaved this way, then again it would be possible for Victor and Peggy to have faked the "experiment", so using a pseudo-random number generator would not reveal  Peggy's knowledge to the world in the same way using a flipped coin would. 

> Digital에서는 무작위로 난수 생성하는 경우에도 고정된 패턴이 존재 할 수 있기 때문에 실험 조작 가능성 존재 함
>
> EX) seed값이 고정된 경우 항상 같은 난수만 나옴

Notice that Peggy could prove to Victor that she knows the magic word, without revealing it to him, in a single trial. If both Victor and Peggy go together to the mouth of the cave, Victor can watch Peggy go in through A and come out through B. This would prove with certainty that Peggy knows the magic word, without revealing the magic word to Victor. However, such a proof could be observed by a third party, or recorded by Victor and such a proof would be convincing to anybody. In other words, Peggy could not refute(부인하다) such proof by claiming she colluded(공모하다)  with Victor, and she is therefore no longer in control of who is aware of her knowledge. 

### Definition

A zero-knowledge proof must satisfy three properties: 

1. **Completeness**: if the statement is true, the honest verifier (that is, one following the protocol properly) will be convinced of this fact by an honest prover.
2. **Soundness**: if the statement is false, no cheating prover can convince the honest verifier that it is true, except with some small probability.
3. **Zero-knowledge**: if the statement is true, no verifier learns anything other than the fact that the statement is true.
   - In other words, just knowing the statement (not the secret) is sufficient to imagine a scenario showing that the prover knows the secret.
   - This is formalized by showing that every verifier has some *simulator* that, given only the statement to be proved (and no access to the prover), can produce a transcript that "looks like" an interaction between the honest prover and the verifier in question.

***The third is what makes the proof zero-knowledge.***

Zero-knowledge proofs are not proofs in the mathematical sense of the term because ***there is some small probability***

> soundness : 만약 statement가 false라면 prover는 verifier를 속일 수 없음

- the *soundness error*, that a cheating prover will be able to convince the verifier of a false statement.
- In other words, ***zero-knowledge proofs are probabilistic "proofs"*** rather than deterministic proofs.
  - However, there are techniques to decrease the soundness error to negligibly small values.

>prover가 증명하려는 값을 직접 verifier에게 보낼수 없기 때문에 prover는 증명하고 싶은 값으로 리버싱이 안되는 값을 생성함. 이 값을 해시에 넣어서 verifier에게 리버싱이 가능한 정보 전달
>
>verifier는 prover가 보낸 값을 리버링 하고 이 값을 해시에 넣어 이 해시값으로 비교 함. 이 해쉬값이 같은지로 prover가 알고 있는지 모르고있는지 판단
>
>https://medium.com/decipher-media/zero-knowledge-proof-chapter-1-introduction-to-zero-knowledge-proof-zk-snarks-6475f5e9b17b

### Applications

### Blockchains

It is proposed that ZKPs could be used to guarantee that transactions are valid despite the fact that information about the sender, the recipient and other transaction details remain hidden.



## From Blog post

Based on https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/

---

### Origin of Zero Knowledge

Problems related to interactive proof systems, theoretical systems where a first party (called a ‘Prover’) exchanges 
messages with a second party (‘Verifier’) to convince the Verifier that some mathematical statement is true.

***What happens if you don’t trust the Verifier?***

The specific concern they raised was *information leakage.* Concretely, they asked, how much extra information is the Verifier going to learn during the course of this proof, beyond the mere fact that the statement is true?

Most real systems implement this ‘proof’ in the absolute worst possible way. The client simply transmits the original password to the server, which re-computes the password hash and compares it to the stored value. ***The problem here is obvious: at the conclusion of the protocol, the server has learned my cleartext password.*** Modern password hygiene therefore involves a good deal of praying that servers aren’t compromised.

### What makes it ‘zero knowledge’?

I’ve claimed to you that this protocol leaks no information about Google’s solution. But don’t let me get away with this! The first  rule of modern cryptography is *never to trust people* who claim such things without proof.

Goldwasser, Micali and Rackoff proposed three following properties  that every zero-knowledge protocol must satisfy. Stated informally, they  are:

1. *Completeness.* If Google is telling the truth, then they will eventually convince me (at least with high probability).
2. *Soundness.* Google can *only* convince me *if* they’re actually telling the truth.
3. *Zero-knowledgeness.* (Yes it’s really called this.) I don’t learn anything else about Google’s solution.

We’ve already discussed the argument for completeness. The protocol will eventually convince me (with a negligible error probability), provided we run it enough times. Soundness is also pretty easy to show here. If Google ever tries to cheat me, I will detect their treachery with overwhelming probability.

The hard part here is the ‘zero knowledgeness’ property. To do this, we need to conduct a very strange thought experiment.

### Recap

So let’s recap. We know that ***the protocol is complete and sound,***  based on our analysis above. The soundness argument holds in any  situation where we know that nobody is fiddling with time — that is, the  Verifier is running normally and nobody is rewinding its execution.

At the same time, ***the protocol is also zero knowledge.*** To prove this, we showed that any Verifier program that succeeds in extracting information must also be able to extract information from a protocol run where rewinding is used and *no information is available in the first place.* Which leads to an obvious contradiction, and tells us that the protocol can’t leak information in either situation.

There’s an important benefit to all this. Since it’s trivial for anyone to ‘fake’ a protocol transcript, even after Google proves to me that they have a solution, I can’t re-play a recording of the protocol transcript to prove anything to anyone else (say, a judge). That’s  because the judge would have no guarantee that the video was recorded honestly, and that I didn’t simply *edit* in the same way Google might have done using the time machine. This means that protocol transcripts themselves contain no information. ***The protocol is only meaningful if I myself participated, and I can be sure that it happened in real time.***