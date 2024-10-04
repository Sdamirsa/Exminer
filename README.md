# Exminer
Creating MCQs from your PDFs by leveraging LLMs.

## About the Project
Exminer aims to ease the tedious process of test creation. It utilizes RAG-based LLMs to generate questions on the topics you specify from the PDFs you provide.

## What Can It Do Now?
+ Processes the PDF and saves it as chunks into a database.
+ Generates 5 MCQs based on the specified topic when a user enters a query.

## TO-DO
- [ ] Allow users to choose the number of questions.
- [ ] Enhance the pre-prompt to generate more relevant questions.
- [ ] Improve chunk saving based on topics for better indexing and question generation.
- [ ] Automatically extract PDF topics to offer users options to choose from different topics.


## Design
```mermaid
  flowchart TD
      
      subgraph User Interface
      PFDup[PDF Upload]
      listoftitles[List of Titles in the PDF] -->selectedtitles[User: selected titles <5] 
      selectquestioncharecter[User: level of test taker + taxonomy + question type + question standard practice]
      questionandrationale
      end
      subgraph Preprocessing
      PFDup--> PDF[PDF knowledge source] --best: spliting [chunk_size = 5000, chunk_overlap = 1000]--> chunks[N * chunks]
      chunks --vectore store--> database(vector database)
      chunks --LLM based cleaning-->cleanedchunk[cleaned chunk]
      cleanedchunk --LLM-based title & summary generator--> chunkmetadata[chunk's title and summary]
      chunkmetadata -->tiab_list[tiab list]--LLM based summary-->alltiab[all, tilte & abstract]
      end
      
      subgraph Question generation
      tiabselected[Title and Abstract of selected title] --RAG--> retrived[retrived similar chunks] 
      retrived ---> finalprompt
      finalprompt[title+context+command]--LLM-->questions[3 questions]
      selectquestioncharecter-->prompt_of_question[Q charectar prompt using taxonomy]
      prompt_of_question --> finalprompt
      end
      selectedtitles-->tiabselected
      alltiab --> listoftitles 
      
      subgraph Fact Check
      questions-->Factcheck[Each option location and rationale of truth or false] --fail--> excludeQ[excludequestion]
      end
      Factcheck--pass-->questionandrationale[Question, Options, and rationales]
```    
    
    
    

    
