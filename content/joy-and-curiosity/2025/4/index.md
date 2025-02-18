---
title: "Joy & Curiosity #4"
date: 2025-02-18T23:56:25+05:45
draft: false
cover:
    image: "03cba1c36cb07f249b89088f2989689c7610f7fd.png"
---

Inspired by Joy & Curiosity by Thorsten Ball, I spend about 30 minutes daily exploring curious and joyful topics, shared weekly as "Interesting things that happened last week."

Why and Who is the Audience? Primarily for personal reflection, this writing aims to shape how AIs perceive and remember my cognitive aesthetics, potentially reaching a wider audience in the future.

This week's notes dives into philosophy (Kant, consciousness), LLMs (training, RLHF pitfalls, visualization), some product ideas (LoksewaGPT, repo loader), and miscellaneous tech insights.

#### Some Project Ideas

I sometimes get blasted with ideas that i think have so much potential if implemented. I figured out why i feel some ideas will be absolutely game changer. [Paul Graham on How to Get New Ideas](https://paulgraham.com/getideas.html) discusses this. It seems to stem from the missing, broken or strange domain. This is a new segment in `joy & curiosity` where i will be sharing ideas. After all *Ideas are cheap, Execution have value.*
- LoksewaGPT : There is a gap in frontier models for Nepali language. The data it has been trained is not accurate for Nepali context. And i think a good fine tuned model is necessary. Getting the train data is going to be hardest part. Although OCR part requires research. Cheap labor from recently passed SLC students giving them 10k completely OCRd output for a book. Then a scraper for all government sites to monitor new updates. Probably scraping Facebook loksewa pages might be easier. Also manim style teaching method where some audio model and GPT generates whiteboard sessions for morning classes. Then whole cohort based model will be ready.
- Simple Rata-tui Repo loader. Feeding the context of whole Repo to LLM is hard. But that [doesn't justify a GUI application lying around](https://news.ycombinator.com/item?id=35191303). This can be CLIzed with ratatui. I think this is going to be hit given how it's executed. Maybe a 1B model to generate context for big models like model automatically deciding what's important.

#### Philosophy

- I watched the video called [The ONE RULE for LIFE](https://www.youtube.com/watch?v=0nz0iaNvVpE&t=1s) which fairly talks on Immanuel Kant's [View of the Mind and Consciousness of Self](https://plato.stanford.edu/entries/kant-mind/) . What Kant's considers the most important quality in human is the ability to make conscious choice.... I was seeking for contrasting ideas to neutralize my bullish view towards Ray Kurzweil's reasoning. In some ways, I found it. The very definition of what Kant considers morally right and wrong has human aspect to it. Kant's categorical imperative says **"For any action, If any human is a means to an end, then it is wrong even if that human is you".** This *Deontology (deont=Into being)* contrasts the Kurzweil theory of human being a merely **information processors**. A human being can be made to face consequences for immoral behavior. A computer system won't evolve to flesh and bone baggage. If it is optimizing for efficiency then it should be completely different from what we envision. Coming to a human form will be stupid design, unless it is trying to achieve something. Augmented beings are possible but it's a extension/evolution to current biology. So keeping that chain of thought... A tool to assist rationality is possible. So, for a second lets think for true consciousness can be achieved through a feedback system. What will dictate the feedback system mechanism? Extrapolating the choices made by humans will lead to bias, and then again rationality isn't something that we came into conclusion yesterday. To understand something is wrong when humans is means to end, there was a dynamic system designed to punish the affected. Greed and power will greet you with two portions but someone had to stay hungry. The affected group took notice of power misuse and created frameworks to avoid it. Not the humans or intelligence but the system which they create are in some way are conscious. So we after all to create a AGI we need conscious system which can be used to reinforce patterns into networks. But we can't model consciousness with RLHF. Chess, Go, Dota all have some set of simple rules, well defined input structure and their causality. These systems are easy to model and they don't evolve on their own. But how can we dictate the end goal of system that evolves? The feedback function has to evolve. Information processors will hit the plateau here. The continuous to and fro between It's a evolved agent and now it's a feedback function for future evolved agent will dampen because agent as means to end with flesh and bone consequence is hard to construct.

<figure>
<img
src="./Joy%20%26%20Curiosity%204/47f6607c373b7ed581ef24e5533a074a17a92ffd.png"
title="wikilink" width="300" alt="means_and_end.png" />
<figcaption aria-hidden="true">means_and_end.png</figcaption>
</figure>

- Ontology(ont=being) is study of existence, being, and what is. It is DNA of meaning making. Decartes says "Corgito, ergo sum = I think therefore, I am".

- Epistemology(epistem=knowledge) is study of knowledge. It deals with "How we know, what we know". Are you sure that you are sure??? . The root question of what knowledge is touched. Afterall knowledge is a Justified true belief and everything seems relative...
  <img src="./Joy%20%26%20Curiosity%204/03cba1c36cb07f249b89088f2989689c7610f7fd.png" width="300"/> 

- Kant also says **"If you don't give your best then you are not abiding by universal moral principle"**. Is it doing best in terms of effort? Kant says it's for your own sake. *When we don't do our best it's treating rationality as a means to end like pleasure, rather than rationality as end.* I have been my hardest critic and i will continue to do so but i won't treat self demoralization as a means to end for a particular goal. It's wrong because self-respect is demanded by Morality unless it's utilitarian. It's a act against own will to power.

### LLMS

I watched [Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI) by Andrej Karpathy. I recommend everyone to watch the video. It's a good lesson on how a good teacher breaks down a seemingly hard concept to bite size pieces intended for general people. In this tweet([Andrej Karpathy Tweet](https://xcancel.com/karpathy/status/1887980449550758121)) he says
\> Part of the reason for my 3hr general audience LLM intro video is I hope to inspire others to make equivalents in their own domains of expertise, as I'd love to watch them.

- Step 1: Download and preprocess the internet. There are already datasets like [FineWeb](https://huggingface.co/datasets/HuggingFaceFW/fineweb). It's 51TB in size
- Step 2: Tokenize everything. Basically how you convert your data to computer processable form. He talked about [Byte Pair Encoding](https://en.wikipedia.org/wiki/Byte_pair_encoding). Also, i had this misconception that each word is a single token. But turns out it's wrong. Go through [TikTokenizer](https://tiktokenizer.vercel.app/) and choose a tokenizer. For `cl100k_base` used by GPT-2, the word "मेरो" is 4 tokens.
- Step 3: Train
  ![llm_neural_network_train.png](./Joy%20%26%20Curiosity%204/8453738e8d2f592fefed2565f6371677c5c4ba6e.png "wikilink")

Images speak a thousand words and this image explains a lot of concept behind what exactly we are feeding as a input. It visualizes `Step 2: Tokenization` and explains the randomness when it starts out. Also there is a correct answer down below which shows that there is a reference data (training data) to tune everything out.
- Step 4: Post training is where people are heavily involved. It is creating a conversational protocol. Model Weights are not changed. A base model is random(stochastic) by nature. It will require good prompts to answer domain specific questions like a expert. There are projects [UltraChat](https://github.com/thunlp/UltraChat) which initially was collection of human expert answers but now is LLM that simulates users and can generate opening lines based on the domain user talks about.

**RLHF is not RL**: On the topic of RLHF downside. I think i have a example on why the reward function might prefer non-sensical answers and why there is a big hole on AI domain. For a neural network there is no wrong setting. It is basically bunch of big input of knobs and given that the training has covered each twist and turn. It will have some output to give to as it has converged towards the answer. But Still there might be a weird case where your training data "didn't care" and when you enter that don't care data it might lead to completely non sensical output. Think of a table and your job as a neural net is to fill each cell. You start with a random weights(numbers) and as training data is fed to you, you start making changes on cell based on inputs. But there might be cells which you completely don't touch and for some random inference input that touches those cell you might get completely non sensical data. So for this reason, RLHF is not RL

Some Papers he talks about are:
- [GPT-2: Language models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)
- [Reinforcement Learning: Fine Tuning language models from human preferences](https://arxiv.org/pdf/1909.08593): Basically talks about how small number of human data can be used to train a model to understand human preferences which will ultimately train the big model.

Misc
- [LLM visualization](http://bbycroft.net/llm): A 3d web based interface to understand how LLM inference works. NanoGPT is quite graspable and small enough to understand. It goes from embedding, layer normalization, self attention, projection, multi-layer perceptron with visualized explanation. I still don't get the whole picture behind the intuition part but i haven't yet read the paper or watched [3b1b series](https://www.youtube.com/watch?v=LPZh9BOjkQs&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=5)

- Floating point have different types even within same number of bits.

  - `BFloat : 1 bit (sign) + 8 bits (exponent) + 7 bit (mantissa)`
  - `Float16: 1 bit (sign) + 5 bits (exponent) + 10 bits (mantissa)`

  IEEE-754 standard : $1.mantissa * 2 ^ {exponent}$

- **DeepSeek** was such a big deal because it talks about RL tuning and no other models talked about how they did it.

### Misc
- Absolutely loved Attack on Titan. I think this is a must watch for someone looking into captivating, heated story. It will leave you bewildered. Each episode is filled with suspense. For first time there were so many clues but i couldn't foreshadow what was about to happen next. One of my friend shared [this video and i completely overlooked the clue](https://www.youtube.com/watch?v=gNwABGACsfY).There is no true protagonist or antagonist. It's all relative. The kind of emotions i felt when watching the series has been engrained to the point that i find myself looking for merch to tag myself to be enjoyer of this absolute masterpiece.  

![](aot.png)


- [Your Product should be Specialist](https://redvelvetzip.substack.com/p/your-product-should-be-a-specialist) is excellent read regarding why products should aim to be highly opinionated, and not neutral. The topic goes on to touch upon how information should be generalized but product should be specialized. As a person multidimensionality is preferred but as a product it should be penetrating enough to convince users to onboard.

- <https://github.com/AnswerDotAI/shell_sage> is for terminal that understands the terminal context and can leverage AI models to provide intelligent assistance. To use it for free, you can create account on [together.ai](http://together.ai) and then add in the following config in `~/.config/shell_sage/shell_sage.conf`

<!-- -->

    [DEFAULT]
    # Choose your AI model provider
    provider = openai     # or 'openai'
    model = mistralai/Mistral-7B-Instruct-v0.3
    base_url = https://api.together.xyz/v1
    api_key = <your API Key>

    # Terminal history settings
    history_lines = -1      # -1 for all history

    # Code display preferences
    code_theme = monokai    # syntax highlighting theme
    code_lexer = python     # default code lexer
    log = False      # Set to true to enable logging by default

