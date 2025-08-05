# Data Collection Protocols

## Survey Study Design

### Target Population
- **Primary**: General reading public (ages 18-65)
- **Secondary**: Creative professionals (writers, editors, publishers)
- **Tertiary**: Academic researchers (literature, AI, philosophy)

### Sample Size Calculation
- **Effect size**: Medium (Cohen's d = 0.5)
- **Power**: 0.80
- **Alpha level**: 0.05
- **Required n**: 128 per group (total ~500 for multiple comparisons)

### Survey Instruments

#### Section A: Demographics & Reading Habits
- Age, education, profession
- Reading frequency and preferences
- Familiarity with AI technology
- Creative writing experience

#### Section B: Narrative Evaluation
- **Randomized presentation**: 4 stories (2 AI, 2 human, blinded)
- **Measures**: 
  - Authenticity scale (1-7 Likert)
  - Emotional engagement (validated scale)
  - Quality assessment (originality, coherence, style)
  - Authorship confidence ratings

#### Section C: Ethical Perspectives
- Acceptability of AI-generated content
- Disclosure preferences
- Economic impact concerns
- Cultural value questions

### Distribution Strategy
- **Online panels**: Prolific Academic, MTurk
- **Social media**: Reddit (r/books, r/writing), Twitter, Facebook groups
- **Professional networks**: LinkedIn writing groups, academic mailing lists
- **Incentives**: $5 compensation for 15-minute completion

## Expert Interview Protocol

### Participant Recruitment
- **Authors**: Published fiction writers (traditional & indie)
- **Publishers**: Editors from major publishing houses
- **Technologists**: AI researchers working on creative applications
- **Ethicists**: Philosophers specializing in technology ethics
- **Critics**: Literary critics and book reviewers

### Interview Structure (45-60 minutes)

#### Opening (5 minutes)
- Background and current work
- Experience with AI writing tools

#### Core Questions (35 minutes)
1. **Authenticity & Value**
   - "What makes a story authentic to you?"
   - "How do you evaluate the value of AI-generated narratives?"

2. **Creative Process**
   - "How might AI change the creative writing process?"
   - "What are the benefits and risks you see?"

3. **Economic & Professional Impact**
   - "How might AI affect your profession/industry?"
   - "What adaptations do you foresee being necessary?"

4. **Ethical Considerations**
   - "What ethical guidelines should govern AI storytelling?"
   - "How should AI-generated content be disclosed?"

5. **Future Scenarios**
   - "What's your vision for AI-human creative collaboration?"
   - "What concerns you most about this technology?"

#### Closing (10 minutes)
- Additional thoughts
- Recommendations for other experts
- Follow-up consent

### Analysis Framework
- **Coding scheme**: Deductive (theory-based) + inductive (emergent themes)
- **Software**: Atlas.ti or NVivo
- **Validation**: Inter-coder reliability (Cohen's κ > 0.80)

## Content Analysis Protocol

### Corpus Selection
- **AI-generated**: 100 stories from GPT-4, Claude, Gemini (various prompts)
- **Human-written**: 100 matched stories (genre, length, publication date)
- **Matching criteria**: Word count (±20%), genre, narrative structure

### Analysis Dimensions

#### Linguistic Features
- **Lexical diversity**: Type-token ratio, Herdan's C
- **Syntactic complexity**: Mean sentence length, embedding depth
- **Semantic coherence**: Topic modeling, coherence scores

#### Narrative Structure
- **Plot elements**: Exposition, rising action, climax, resolution
- **Character development**: Complexity, consistency, agency
- **Thematic content**: Automated theme extraction, human validation

#### Stylistic Markers
- **Voice consistency**: Personal pronouns, modal verbs
- **Figurative language**: Metaphor detection, imagery analysis
- **Cultural references**: Named entity recognition, cultural specificity

### Technical Implementation
```python
# Example analysis pipeline
import nltk, spacy, transformers
from textstat import flesch_reading_ease
from sklearn.feature_extraction.text import TfidfVectorizer

def analyze_narrative(text):
    # Linguistic features
    readability = flesch_reading_ease(text)
    
    # Semantic analysis
    doc = nlp(text)
    entities = [(ent.text, ent.label_) for ent in doc.ents]
    
    # Stylistic features
    sentences = sent_tokenize(text)
    avg_sentence_length = np.mean([len(word_tokenize(s)) for s in sentences])
    
    return {
        'readability': readability,
        'entities': entities,
        'avg_sentence_length': avg_sentence_length
    }
```

## Focus Group Protocol

### Group Composition
- **Size**: 6-8 participants per group
- **Variation**: Age, education, reading habits
- **Total**: 8 groups (64 participants)

### Session Structure (90 minutes)

#### Warm-up (15 minutes)
- Introductions
- Reading preferences discussion

#### Activity 1: Blind Evaluation (30 minutes)
- Read 3 short stories (mixed authorship)
- Individual ratings and notes
- Group discussion of preferences

#### Activity 2: Revelation & Discussion (30 minutes)
- Reveal which stories were AI-generated
- Discuss reaction to revelation
- Explore reasoning behind preferences

#### Activity 3: Future Scenarios (15 minutes)
- Present hypothetical scenarios
- Discuss acceptability and concerns
- Identify deal-breakers and opportunities

### Moderation Guidelines
- **Facilitator training**: 2-hour workshop on bias mitigation
- **Recording**: Audio with participant consent
- **Transcription**: Professional service with confidentiality agreement
- **Analysis**: Thematic analysis with multiple coders