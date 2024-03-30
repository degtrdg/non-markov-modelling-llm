# Data

- there are n tokens in vocabulary
- These are the rules for the dataset
  - only appearing in multiples of m
    - 2 <= m <= 5
    - make sure it is consistent
  - char if you place it will copy the one right behind it
- <s> <e>

## Example

- parameters
  rule = A with multiples of 2, B with multiples of 3
  n = 2
  l_c = 5
  l_t = 10
  m_a = 2
  m_b = 3
- result
  ctx = AAABB
  gen = AAABB

- rules that don't work together example
  - A's have to be a multiple of 4 and B's have to copy 3 letters behind it

## Model

- we'll get llama, mistral, and others finetune, get metric of based models ability to do non-markov modeling
- just use the huggingface stuff to make it work fast

- input
  - <s> data <e> ...
-

Training

Evaluation

- within the same ruleset, you have enough sampled instances that are correct to show that it statistically knows the rule

  - paramerters include
    - size of the rule set
      - you can't just guess it
    - length of the generation
    - how many of the same ruleset you use
  - make sure that the context doesn't always end with a valid sequence so that i can't copy and paste

- Rule set should be visualized as a type of graph
  - Example with A,B,C,D
    - B only appears in multiples of 3
    - If DC appears, then A is next
    - A has a .2, .3, .5 probability of selecting B,C,D next
    - C has a .3, .7 probability of selecting D,A next
    - The last B in the multiple will randomly select between C and D
    - D has a .2,.8 probability of selecting C,B next
  - The graph will look like this:
    A
    |--.2--> B --> B --> B
    | |--.5--> C
    | |--.C--> D
    |
    |--.3--> C --.3--> D
    | |---.7--> A
    |
    |--.5--> D
    |--.2--> C --> A
    |--.8--> B
  - Finding yourself at any node, follow the graph and according to the probabilities (if no probability is indicated
    then the probability of that path is 1), and if you are at a "leaf" with no outgoing arrow, then go back to the first
    A and go to the first instance of that letter.
