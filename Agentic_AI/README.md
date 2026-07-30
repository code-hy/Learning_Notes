# Initial Notes

## Agentic Design Patterns
1. Reflection - check code correctly for correctness, style and efficiency, and give constructure critism for how to improve it
2. Tool use - web search tool, code execution tool etc
3. Planning - please generate an image where a girl is reading a book and her pose is the same as the boy in the image example.jpg, then please describe the new image with with your voice
4. Multi-agent collaboration

### Tool Use
* Analysis
  - Code execution
  - Wolfram Alpha
  - Bearly code interpreter
* Information Gathering
  - Web search
  - Wikipedia
  - Database Access
* Productivity
  - Email
  - Calendar
  - Messaging
* Images
  - Image generation
  - Image captioning
  - OCR

### Planning
 <img width="1322" height="601" alt="image" src="https://github.com/user-attachments/assets/0a3ccce4-510c-48f0-a752-8415110b4774" />

### Multi-agentic Workflows
<img width="1151" height="528" alt="image" src="https://github.com/user-attachments/assets/7a14df00-5ae3-48a8-bf07-3d259d805b4e" />

## Reflection Design Patterns
* reflection with external feedback (ie. run the code)
* <img width="1350" height="539" alt="image" src="https://github.com/user-attachments/assets/3adbcd9b-570e-44d0-9728-7768eade050c" />
* Why use reflection pattern, rather than direct generation
  - zero-shot prompting (no examples)
  - One-shot (single example)
  - Two-shot (two examples)
  - Multi-shot (more examples)
  
| Example | Problem | Reflection prompt |
| ---------- | ----------- |---------------|
| Generate html table | Missing '>' | Validate the html code |
| How to brew a perfect cup of tea | Missing steps | Check instructions for coherence and completeness |
| Generating domain names | Name has unintended meaning, or is hard to pronounce | Does domain name have any negative connotations? Is the domain name hard to pronounce? |

* Brainstorming domain names
  - review the domain names you suggested
  - check if each name is easy to pronounce and thus easy to spread via word of mouth
  - consider whether each name might mean something negative in other languages
  - then output a shortlist of only the names that meet these criteria

* Improving email
  - review the email first draft
  - check that the tone is professional and look for phrases that could be considered rude or insensitive
  - verify all facts, dates, and promises are accurate.
  - then write the next draft of the email.

<img width="1304" height="631" alt="image" src="https://github.com/user-attachments/assets/16a53e0e-f669-4cee-82c3-07366fb638af" />

 * clearly indicate the reflection action
 * specify criteria to check

### Chart Generation Workflow

<img width="1331" height="704" alt="image" src="https://github.com/user-attachments/assets/5aa7328f-9a13-4e79-99e7-ac7dd9fa74df" />

   <img width="1335" height="687" alt="image" src="https://github.com/user-attachments/assets/e2d89b4c-7427-4e6f-b136-78470bc2c2ba" />
 
### Evaluating the impact of reflection
**Create a dataset of prompts and answers - this is technique used in datatalks llm-zoomcamp)**
<img width="1239" height="687" alt="image" src="https://github.com/user-attachments/assets/6b2d047b-1e28-46ab-b00e-9462377b4450" />

_run each time you change the reflection prompt_

<img width="1024" height="742" alt="image" src="https://github.com/user-attachments/assets/951f4bf9-83df-4715-89df-522bff15d83a" />

```mermaid
flowchart LR
    %% Node Definitions
    A(["Which color of product has the highest total sales?"]) --> B[LLM]
    
    subgraph Reflection_Step ["Reflect on V1 SQL, write V2 query"]
        C[LLM]
    end

    D["execute SQL"]
    E["V2 query results"]
    F[LLM]
    G([Answer question])

    %% Connections
    B -- "Generate SQL query" --> C
    C --> D
    D --> E
    E --> F
    F --> G

    %% Styling
    style A fill:#fbebeb,stroke:#d9534f,stroke-width:1px,color:#000
    style Reflection_Step fill:none,stroke:#d9534f,stroke-width:2px,stroke-dasharray: 5 5
    style D fill:#eafaf1,stroke:#27ae60,stroke-width:1px,color:#000
```

### Reflection results

Comparison of having ground truth, no reflection, and with reflection (the reflection prompt is - reflecton V1 SQL , and write V2 SQL)
This is for objective tasks

| Prompts | Ground truth answer | No reflection | With reflection |
| :--- | :--- | :--- | :--- |
| **Number of items sold in May 2025?** | 1201 | 980 | 1201 |
| **Most expensive item?** | Airflow sneaker | Airflow sneaker | Airflow sneaker |
| **How many styles carried?** | 14 | 14 | 14 |
| **Accuracy Rate** | — | **87% correct** | **95% correct** |

This is for subjective tasks (by a human or use LLM to judge)
<img width="932" height="480" alt="image" src="https://github.com/user-attachments/assets/7be3f090-6ae8-48be-ac2f-4cf8c2e17206" />

<img width="880" height="400" alt="image" src="https://github.com/user-attachments/assets/57bc1653-1783-48c4-8909-304fa4231f10" />

Grading with a rubric gives more consistent results - like scores binary 1 or 0 for each criteria and then add them all up.

<img width="890" height="389" alt="image" src="https://github.com/user-attachments/assets/5bed68c3-bc0d-4ece-8161-5710babfecd9" />

<img width="832" height="486" alt="image" src="https://github.com/user-attachments/assets/b2f2be9c-c785-42df-9c59-3118d323cbaa" />

#### Evaluating reflection
* Objective evals
  - Code-based evals are easier
  - Build a dataset of ground truth examples
* Subjective evals
  - Use LLM as a judge
  - Rubric-based grading is better
 
### Reflection using external feedback
<img width="1320" height="646" alt="image" src="https://github.com/user-attachments/assets/7ec42b48-d1db-449d-9053-de818cff0dbb" />

<img width="902" height="417" alt="image" src="https://github.com/user-attachments/assets/225b3e45-4539-4c3f-a35f-6eaf9fd8b291" />

#### Other examples of tools to help reflection

| Challenge | Example | Source of Feedback |
|:--- | :--- | :--- |
| Mentioning competitors | Our company's shoes are better than RivalCo | Pattern matching for competitor names |
| Fact checking an essay | The Taj Mahal was built in 1648 | Web search results |
| LLM won't follow output length guidelines | Essay is over word limit | Word count tool |

## Tool Use

Function to call
<img width="894" height="430" alt="image" src="https://github.com/user-attachments/assets/c7d75497-bed4-4a1b-844b-75a211f87415" />

### Examples
| Prompt | Tool | Output |
| :--- | :--- | :--- |
| **Can you find some Italian restaurants near Mountain View, CA?** | `web_search(query="restaurants near Mountain View, CA")` | Spaghetti City is an Italian restaurant in Mountain View... |
| **Show me customers who bought white sunglasses** | `query_database(table="sales", product="sunglasses", color="white")` | 28 customers bought white sunglasses. Here they are... |
| **How much money will I have after 10 years if I deposit $500 at 5% interest?** | `interest_calc(principal=500, interest_rate=5, years=10)`<br><br>**OR**<br><br>`eval("500 * (1 + 0.05) ** 10")` | $814.45 |

<img width="916" height="432" alt="image" src="https://github.com/user-attachments/assets/76ab4277-6495-4d6f-99f3-021520bda7e6" />

### Creating a tool

<img width="838" height="312" alt="image" src="https://github.com/user-attachments/assets/9e3316b5-2619-482a-a9e0-ee030bbe4611" />

<img width="888" height="463" alt="image" src="https://github.com/user-attachments/assets/f9e70f28-94d6-45e8-b9b2-4fa43351ec14" />

<img width="890" height="466" alt="image" src="https://github.com/user-attachments/assets/f2b00210-5383-4098-a181-7920fa272477" />

### Tool syntax
<img width="900" height="426" alt="image" src="https://github.com/user-attachments/assets/5f0ca33d-0f53-4027-b218-49b7965a7d6b" />

<img width="889" height="432" alt="image" src="https://github.com/user-attachments/assets/822eb0d9-03ea-45fe-bc88-fb0641724987" />

<img width="900" height="444" alt="image" src="https://github.com/user-attachments/assets/2f2733ca-0e3f-4e49-a3b8-a7a78ce1e17d" />
