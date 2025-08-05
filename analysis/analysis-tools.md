# Analysis Templates and Tools

## Statistical Analysis Framework

### Quantitative Analysis Plan

#### Descriptive Statistics
```r
# Sample R code for survey analysis
library(tidyverse)
library(psych)
library(corrplot)

# Load survey data
survey_data <- read_csv("data/survey_responses.csv")

# Demographic breakdown
survey_data %>%
  group_by(age_group, profession) %>%
  summarise(
    n = n(),
    ai_acceptance_mean = mean(ai_acceptance_scale, na.rm = TRUE),
    authenticity_concern_mean = mean(authenticity_concern, na.rm = TRUE)
  ) %>%
  arrange(desc(ai_acceptance_mean))

# Correlation matrix for key variables
key_vars <- survey_data %>%
  select(ai_acceptance_scale, authenticity_concern, 
         quality_perception, economic_concern, 
         reading_frequency, tech_familiarity)

corrplot(cor(key_vars, use="complete.obs"), 
         method="circle", type="upper")
```

#### Inferential Statistics
- **ANOVA**: Compare AI acceptance across demographic groups
- **Regression**: Predict authenticity concerns from demographic and attitudinal variables
- **Chi-square**: Test independence between categorical variables
- **T-tests**: Compare human vs AI story ratings

### Content Analysis Automation

#### Text Processing Pipeline
```python
import pandas as pd
import nltk
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import KMeans
from textstat import flesch_reading_ease
import spacy

nlp = spacy.load("en_core_web_sm")

def analyze_narrative_corpus(stories_df):
    \"\"\"
    Comprehensive analysis of story corpus
    Input: DataFrame with columns ['text', 'source_type', 'genre']
    Output: Analysis results dictionary
    \"\"\"
    
    results = {}
    
    # Basic statistics
    stories_df['word_count'] = stories_df['text'].str.split().str.len()
    stories_df['sentence_count'] = stories_df['text'].str.split('.').str.len()
    stories_df['readability'] = stories_df['text'].apply(flesch_reading_ease)
    
    # Linguistic features
    def extract_features(text):
        doc = nlp(text)
        return {
            'entities': len(doc.ents),
            'adjectives': len([token for token in doc if token.pos_ == 'ADJ']),
            'verbs': len([token for token in doc if token.pos_ == 'VERB']),
            'unique_words': len(set([token.lemma_ for token in doc]))
        }
    
    linguistic_features = stories_df['text'].apply(extract_features).apply(pd.Series)
    stories_df = pd.concat([stories_df, linguistic_features], axis=1)
    
    # Thematic analysis using topic modeling
    vectorizer = TfidfVectorizer(max_features=100, stop_words='english')
    tfidf_matrix = vectorizer.fit_transform(stories_df['text'])
    
    kmeans = KMeans(n_clusters=5, random_state=42)
    stories_df['theme_cluster'] = kmeans.fit_predict(tfidf_matrix)
    
    # Compare AI vs Human
    comparison = stories_df.groupby('source_type').agg({
        'word_count': ['mean', 'std'],
        'readability': ['mean', 'std'],
        'entities': ['mean', 'std'],
        'unique_words': ['mean', 'std']
    }).round(2)
    
    results['descriptive_stats'] = comparison
    results['theme_distribution'] = stories_df.groupby(['source_type', 'theme_cluster']).size()
    
    return results

# Usage example
# results = analyze_narrative_corpus(story_data)
```

### Survey Analysis Templates

#### Likert Scale Analysis
```r
# Analyze Likert scale responses
library(likert)
library(ggplot2)

# Prepare Likert data
likert_items <- survey_data %>%
  select(authenticity_important, quality_over_source, 
         ai_transparency_needed, economic_concern_level) %>%
  mutate_all(~factor(., levels=1:7, 
                     labels=c("Strongly Disagree", "Disagree", "Somewhat Disagree",
                              "Neutral", "Somewhat Agree", "Agree", "Strongly Agree")))

# Create Likert object and plot
likert_obj <- likert(likert_items)
plot(likert_obj) + 
  theme_minimal() +
  labs(title="Attitudes Toward AI Storytelling",
       subtitle="Distribution of Responses (n=500)")
```

#### Regression Analysis Template
```r
# Multiple regression predicting AI acceptance
model1 <- lm(ai_acceptance_scale ~ age + education + tech_familiarity + 
             reading_frequency + profession + economic_concern, 
             data = survey_data)

summary(model1)

# Model diagnostics
par(mfrow=c(2,2))
plot(model1)

# Effect sizes
library(lsr)
etaSquared(model1)
```

## Qualitative Analysis Framework

### Interview Coding Scheme

#### Deductive Codes (Theory-Based)
1. **Authenticity Concepts**
   - Personal expression
   - Cultural authenticity
   - Emotional genuineness
   - Originality

2. **Economic Concerns**
   - Job displacement
   - Market competition
   - Compensation fairness
   - Industry transformation

3. **Ethical Considerations**
   - Transparency needs
   - Consumer rights
   - Creator protection
   - Cultural preservation

#### Inductive Coding Process
```
1. Initial Reading: Familiarization with data
2. Open Coding: Identify emerging themes
3. Axial Coding: Develop relationships between themes
4. Selective Coding: Integrate into core categories
5. Theoretical Integration: Connect to existing literature
```

### Thematic Analysis Template
```
Theme: [Theme Name]
Definition: [Clear definition of theme]
Prevalence: [How many participants mentioned this]
Subthemes:
  - Subtheme 1: [Description]
  - Subtheme 2: [Description]
Example Quotes:
  - "Quote 1" (Participant ID, Role)
  - "Quote 2" (Participant ID, Role)
Connections to Literature: [Relevant research]
Implications: [Practical and theoretical implications]
```

## Mixed-Methods Integration

### Convergent Design Analysis
```r
# Quantitative results
quant_summary <- survey_data %>%
  summarise(
    mean_ai_acceptance = mean(ai_acceptance_scale),
    mean_authenticity_concern = mean(authenticity_concern),
    correlation_quality_source = cor(quality_rating, source_preference)
  )

# Qualitative themes (manual coding results)
qual_themes <- data.frame(
  theme = c("Authenticity Central", "Quality Trumps Source", "Economic Anxiety"),
  prevalence = c(0.85, 0.60, 0.70),
  valence = c("negative", "neutral", "negative")
)

# Integration analysis
# Compare quantitative means with qualitative theme prevalence
# Look for convergence, divergence, or complementarity
```

### Joint Displays
```r
# Create joint display table
library(knitr)
library(kableExtra)

joint_display <- data.frame(
  "Research Question" = c("RQ1: Authenticity Impact", "RQ2: Quality Perception", "RQ3: Economic Concerns"),
  "Quantitative Finding" = c("High authenticity concern (M=5.2/7)", 
                            "No significant quality difference", 
                            "Moderate economic worry (M=4.1/7)"),
  "Qualitative Finding" = c("Authenticity seen as core value (85% of participants)",
                           "Quality evaluation independent of source awareness",
                           "Professional displacement anxiety prominent"),
  "Integration" = c("Convergent: Both methods show authenticity priority",
                   "Complementary: Quantitative shows 'what', qualitative shows 'why'",
                   "Convergent: Consistent concern levels across methods")
)

kable(joint_display) %>%
  kable_styling(bootstrap_options = c("striped", "hover"))
```

## Visualization Templates

### Survey Results Visualization
```r
# Demographic breakdown visualization
ggplot(survey_data, aes(x = age_group, y = ai_acceptance_scale, fill = profession)) +
  geom_boxplot() +
  facet_wrap(~reading_frequency) +
  theme_minimal() +
  labs(title = "AI Acceptance by Demographics and Reading Habits",
       x = "Age Group", y = "AI Acceptance (1-7 scale)")

# Correlation heatmap
library(corrplot)
corr_matrix <- cor(survey_data[,c("ai_acceptance_scale", "authenticity_concern", 
                                 "quality_perception", "economic_concern")], 
                  use="complete.obs")
corrplot(corr_matrix, method="color", type="upper", 
         addCoef.col = "black", tl.col="black")
```

### Qualitative Results Visualization
```r
# Word cloud from interview transcripts
library(wordcloud)
library(tm)

# Prepare text data
interview_corpus <- Corpus(VectorSource(interview_transcripts$text))
interview_corpus <- tm_map(interview_corpus, content_transformer(tolower))
interview_corpus <- tm_map(interview_corpus, removePunctuation)
interview_corpus <- tm_map(interview_corpus, removeWords, stopwords("english"))

# Create word cloud
wordcloud(interview_corpus, max.words=100, random.order=FALSE, 
          colors=brewer.pal(8,"Dark2"))

# Theme prevalence chart
theme_data <- data.frame(
  theme = c("Authenticity", "Quality", "Economics", "Transparency", "Creativity"),
  prevalence = c(0.85, 0.60, 0.70, 0.90, 0.45)
)

ggplot(theme_data, aes(x=reorder(theme, prevalence), y=prevalence)) +
  geom_col(fill="steelblue") +
  coord_flip() +
  theme_minimal() +
  labs(title="Qualitative Theme Prevalence", 
       x="Theme", y="Proportion of Participants")
```

## Reproducibility Checklist

### Data Management
- [ ] Raw data stored with timestamps and version control
- [ ] Cleaning procedures documented step-by-step
- [ ] Missing data patterns analyzed and reported
- [ ] Outliers identified and decisions documented

### Analysis Documentation
- [ ] All analysis code commented and organized
- [ ] Package versions and dependencies recorded
- [ ] Random seeds set for reproducible results
- [ ] Sensitivity analyses conducted and reported

### Reporting Standards
- [ ] Sample sizes reported for all analyses
- [ ] Effect sizes calculated and interpreted
- [ ] Confidence intervals provided where appropriate
- [ ] Assumptions tested and violations addressed