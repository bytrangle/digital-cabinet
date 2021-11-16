# Making A Bet On Elixir - DockYard
For many organizations considering a shift to the Elixir programming language, their primary concern is: has it been tried, tested, and proven to be successful? The short answer is a resounding, “yes.” The technical merits of Elixir, and its inherited excellence from Erlang, have been [covered](https://youtu.be/JvBT4XBdoUE) [well](https://underjord.io/the-beam-marches-forward.html), [already](https://www.beamrad.io/7), [in](https://dockyard.com/blog/2021/03/30/elixir-is-safe) [many](https://serokell.io/blog/elixir-in-production-glific) [places](https://serokell.io/blog/elixir-in-production-plausible-analytics). And there are plenty of [Elixir-based companies effectively harnessing the power of Elixir](https://underjord.io/elixir-businesses-doing-well.html) (i.e. Discord, Pepsico, Bleacher Report, Pinterest).

The second concern when considering Elixir is often related to finding the right talent. Common questions include:

-   Can we hire for it?
-   Can our existing developers pick it up?
-   Can we bring in untrained developers to learn it?
-   Can we get further training for our developers?

These are all important questions when a company is looking to maintain the people and the skills they need to scale and sustain a software product or business system.

### Can we hire for it?

The simplest approach to addressing a skills gap on a team is hiring. It’s often a good and effective thing to do. Unfortunately, it is often quite difficult. This isn’t particular to Elixir. Experienced software developers are in high demand, while a lot of junior job postings are swarmed with applicants and require significant effort to identify suitable candidates. Recruiting and hiring software developers is typically not easy. Elixir is not much different in that regard.

In my experience hiring for Elixir developers, the talent pool often has a fairly high skill floor. Developers who pursue Elixir work tend to have more experience than the industry average.

For example, when recruiting for two openings with a senior profile, we found six candidates that could have done the job from a pre-screen of 30 or so applicants. And this was without casting a very wide net. There was active interest in Elixir work from experienced candidates. Alex Koutmos mentions [making that choice himself](https://akoutmos.com/post/betting-on-elixir/) in an article on hiring, teaching, and learning Elixir. Developers look for opportunities that will let you work with it. His article also includes some strong recommendations on places to post jobs.

Actually, let’s unpack that. The Elixir community is fairly cohesive. It doesn’t sprawl in the way some huge ecosystems (Java, PHP, Python, Javascript) do. So you can actually list the primary places to make job postings without involving general sites like Stack Overflow. Alex suggests the following which all rhyme with what I’d suggest:

-   [ElixirForums job section](https://elixirforum.com/c/community/elixir-jobs)
-   [ElixirRadar newsletter](https://elixir-radar.com/jobs)
-   [Elixir Slack, #jobs channel](https://elixir-lang.slack.com/archives/C060RDHPX)

Using any of the above job boards would expose many thousands of Elixir-interested developers to your job posting.

Another consideration is the time it takes to train a developer who is new to Elixir. The common wisdom for how long it takes to get up to speed working on a code base varies immensely–some say a month, some say six months. Some say it depends on the size of the code base, while others say it varies by language.

I’ve [covered my “ridiculously straight-forward” experiences](https://underjord.io/onboarding-to-elixir.html) of onboarding to Elixir and that’s one perspective. Claudio Ortolina has also described [his experience onboarding people](https://claudio-ortolina.org/posts/teaching-elixir/) to Elixir at PSPDFKit. He mentions it took two to three months for various new developers to become proficient contributors who could work on “most areas of the codebase.” The general sentiment is that it doesn’t take a long time for someone to become productive in an Elixir code base compared to other languages.

When it comes to junior postings, I think there is an interesting self-selection effect there as well. To start, you won’t need to sift through nearly as many applications since there aren’t endless Elixir bootcamps churning out new devs every day. And the devs that have locked in on Elixir, even if inexperienced, likely proactively sought it out somewhere. This means they are likely to be a bit more engaged with their tech of choice than the average developer.

All that said, if you focus recruiting efforts on existing Elixir experience, then you may have a smaller pool to choose from compared to some of the more widely-adopted languages. We will get into the alternatives as we go.

Elixir hiring is also well-suited for the rising remote workforce. When possible, I would recommend recruiting for remote Elixir talent. Hiring local devs can work fine, especially in areas with lots of people, however you will have better results on a global or continent-wide search. Or, if hiring local talent is necessary, consider offering good relocation packages or train your existing developers in Elixir and then recruit new developers and train them into your stack. This brings us to the next common Elixir hiring question…

### Can our existing developers pick it up?

Perhaps you’ve decided to transition to Elixir, but your current team is not experienced in using it. What then? Can you train your team to a level of proficiency? A recent BEAM Radio episode, with two people who [brought Elixir into use at Spotify](https://www.beamrad.io/14), sheds a lot of light on both how to go about this process and the experience of learning Elixir. I also recommend reading Sophie DeBenedetto’s post on [how a team new to Elixir over-delivered a project in three months](https://www.thegreatcodeadventure.com/an-elixir-adoption-success-story/). The piece details how an Elixir team and a non-Elixir team partnered to execute on an Elixir project. And being the teacher she is, Sophie covers a lot of ground on how to teach Elixir effectively.

Overall, yes, Elixir is very friendly to learn. It has challenging parts – as all languages do – but the learning curve can be quite gentle. We will cover more learning resources towards the end.

### Can we bring in untrained developers to learn it?

Most companies I’ve dealt with in the Elixir space have had good luck with fairly tight, small-to-medium sized teams. There are, however, companies that are looking to build teams of hundreds of developers. In that scenario, you might find some limitations to the existing pool of developers with Elixir experience, especially on the intermediate-to-experienced end. A fast-paced startup can grow faster than those devs become available.

Across the entire tech sector, there is an ongoing debate around whether to look for experienced developers regardless of language skill or if you should look for someone with particular domain experience – even if they have less overall experience. That’s an unresolved discussion; both approaches can give good results.

Hiring from outside the Elixir skills pool is one approach to tackling unusually high growth requirements. This is exemplified in James Russos’ post about his learnings [onboarding to and hiring for Elixir](https://boredhacking.com/elixir-learnings-and-hiring/) at Brex. He notes that “Brex has hired and onboarded over 150 engineers to write Elixir in a production system” over the last three to four years. This is the type of scale where I would expect that you’d need to look beyond the existing pool of Elixir developers.

Thankfully, this approach has proven to be effective. Patryk Bak covers [a specific case where his team chose to hire untrained Elixir devs](https://patrykbak.com/2021/06/29/do-not-seek-elixir-developer.html) and how they went about training them. Similarly, Claudio’s piece (referenced above) notes at least three instances of [teaching Elixir from scratch](https://claudio-ortolina.org/posts/teaching-elixir/).

### Can we get further training for our developers?

Yeah. You can. For one thing, [DockYard offers training](https://dockyard.com/services/training-and-support).

There are also a number of free resources to help with learning Elixir. To start we have:

-   The great [Elixir language guides](https://elixir-lang.org/getting-started/introduction.html)
-   [The Elixir documentation](https://hexdocs.pm/elixir/Kernel.html)
-   The [Phoenix Guides & Documentation](https://hexdocs.pm/phoenix/overview.html)
-   [The Elixir School curriculum](https://elixirschool.com/en/) (translated to many languages)

And then we have a variety of other organizations that provide training. Groxio, [provides training and courses](https://grox.io/) for programmers with a strong foundation in Elixir; and The Pragmatic Bookshelf, also known as PragProg, offers [a solid library of books in the Elixir space](https://pragprog.com/categories/elixir-phoenix-and-otp/).

These are only some of the well-established actors that speak to the stability of the Elixir teaching and training ecosystem. You can find a large number of smaller consultancies, coaches, and experts all across the community that can work with your particular needs.

### In Conclusion

It is reasonable to ask yourself: Can I find the developers I need if I choose a more niche programming language? The truth is that there are experienced developers ready to pounce on that opportunity out there. You are hardly cutting trail when you choose Elixir. There is a lively, active community right there to walk with you. The path forward might not be a six-lane super highway, but the roads are well-paved and the traffic is much more reasonable.

It is understandable that people might think choosing a language outside the “top 10 most used programming languages” could cause trouble. And there can certainly be unique differences to taking “the road less traveled.” But, on the flip side, you will never find a competitive advantage in picking from the top 10 list either (since that’s statistically what everyone does!).

The proof of Elixir’s potential is out there. Erlang has long since proven that the foundation Elixir builds on is strong. WhatsApp is a great example of that, building [massive usage on a very small team](https://www.wired.com/2015/09/whatsapp-serves-900-million-users-50-engineers/). And Elixir, building on that, has long since proven itself. Discord being [one massive example – they found success with a small team](https://elixir-lang.org/blog/2020/10/08/real-time-communication-at-scale-with-elixir-at-discord/). Their success challenges the misconception that a large team is needed. These teams have likely both grown by now as well.

Why not gain a competitive edge and join the ranks of Elixir success stories?

_DockYard is a digital product consultancy specializing in production-ready apps that scale wonderfully as companies grow. We offer a range of consulting services with capabilities in product planning, design, user experience (UX), full-stack engineering, and QA. Over the last decade, DockYard has solved complex product challenges for visionary companies like Netflix, Apple, Nasdaq, and Harvard. We’re also dedicated to advancing open-source web development technologies, such as libraries and tooling built around the Elixir programming language. From idea to impact, DockYard empowers ambitious teams to build for the future. Visit us at [www.dockyard.com](https://dockyard.com/)._ 
 [https://dockyard.com/blog/2021/11/15/making-a-bet-on-elixir](https://dockyard.com/blog/2021/11/15/making-a-bet-on-elixir) 
 [https://dockyard.com/blog/2021/11/15/making-a-bet-on-elixir](https://dockyard.com/blog/2021/11/15/making-a-bet-on-elixir)
