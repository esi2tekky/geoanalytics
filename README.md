# Brand Visibility in Large Language Models: A Generative Engine Optimization Analysis



## Project Overview

### Research Objective

This project investigates the factors that influence brand visibility and positioning in Large Language Model (LLM) responses, specifically focusing on ChatGPT outputs. By analyzing real user-LLM conversations, we aim to identify actionable strategies for Generative Engine Optimization (GEO) - the emerging practice of optimizing brand presence in AI-generated content.

### Core Research Questions

1. **Position Analysis:** Which brands consistently achieve top positions (1st, 2nd, 3rd) in LLM recommendations, and what factors predict these positions?

2. **Sentiment and Context:** How are brands described in LLM responses, and does sentiment quality correlate with positioning?

3. **Competitive Dynamics:** Which brands appear together in responses, and what does this reveal about competitive landscapes in LLM outputs?

4. **Query Pattern Optimization:** What specific query phrasings, intents, and patterns maximize brand visibility and favorable positioning?

### Scope and Constraints

- **Industries:** Technology, E-commerce, Food/Beverage, Automotive

- **LLM Focus:** ChatGPT models (GPT-3.5-turbo, GPT-4) from LMSYS-Chat-1M dataset

- **Sample Size:** Approximately 200,000 conversations (strategically sampled)

- **Brand Universe:** 50-150 major brands across selected industries

- **Temporal Coverage:** Date range dependent on LMSYS dataset (likely 2023-2024)

### Expected Outcomes

- Quantitative analysis of brand mention patterns and positioning

- Predictive model for brand position in LLM responses

- Actionable GEO recommendations for brand visibility optimization

- Performance analysis of SQL queries at scale

---

## Section 1: Project Overview and Dataset Introduction

### Objectives

Establish the analytical framework and introduce the datasets that will form the foundation of our analysis.

### Approach

#### 1.1 Dataset Acquisition and Initial Exploration

We begin by downloading the LMSYS-Chat-1M dataset from HuggingFace, which contains real user conversations with various LLM models. This dataset is valuable because it represents authentic user queries and LLM responses in natural settings, not artificially constructed test cases.

**Key steps:**

- Download LMSYS-Chat-1M dataset via HuggingFace datasets library

- Filter specifically for ChatGPT models (gpt-3.5-turbo, gpt-4, etc.) to ensure consistency

- Examine dataset structure: conversation format, available metadata, temporal coverage

- Assess data quality: completeness of conversations, response lengths, metadata availability

#### 1.2 Strategic Sampling

Given the large dataset size, we will implement strategic sampling to create a manageable yet representative subset.

**Sampling criteria:**

- Target sample size: 200,000 conversations

- Random sampling with fixed seed for reproducibility

- Verify representation across time periods

- Ensure adequate conversation length (filter out very short exchanges)

- Check for diversity in query types

**Rationale:** 200,000 conversations should provide sufficient brand mentions for statistical significance while remaining computationally manageable within BigQuery cost constraints.

#### 1.3 Conversation Structure Analysis

Understanding how conversations are structured is critical for extracting relevant information.

**Analysis components:**

- Identify conversation turn structure (user/assistant alternations)

- Extract user queries (typically first user turn)

- Extract assistant responses (first assistant turn following user query)

- Measure response characteristics: length, complexity, formatting

- Document any conversation patterns that might affect brand mentions

#### 1.4 Dataset Assessment

Before proceeding with brand extraction, we need to assess whether our sampled dataset contains sufficient brand-related content.

**Assessment metrics:**

- Percentage of conversations mentioning products, services, or companies

- Distribution of conversation topics

- Prevalence of recommendation, comparison, and informational queries

- Preliminary keyword analysis for major brand names

**Decision point:** If brand-related content is insufficient (less than 10% of conversations), we will supplement with WildChat dataset following the same filtering and sampling approach.

### Expected Deliverables

- Comprehensive dataset statistics: conversation counts, date ranges, model distribution

- Sample conversation examples demonstrating data structure

- Initial assessment of brand mention potential

- Clear documentation of sampling methodology and rationale

### Writeup Components

- Dataset provenance and selection rationale

- Sampling methodology and reproducibility measures

- Initial data quality assessment

- Scope limitations and data constraints

---

## Section 2: Brand Identification and Supplementary Data Integration

### Objectives

Extract brand mentions from LLM responses with positional and contextual information, then enrich this data with external metrics that proxy for brand awareness and market presence.

### Approach

#### 2.1 Brand Universe Definition

Create a comprehensive list of brands to track across our four target industries.

**Industry-specific brand selection:**

**Technology:**

- Consumer devices: smartphones, laptops, tablets (Apple, Samsung, Google, Dell, HP, Lenovo, etc.)

- Software and cloud services: Microsoft, Adobe, Salesforce, AWS, Azure, Google Cloud

- Social media and platforms: Meta, Twitter, TikTok, LinkedIn, Reddit, Discord

- Entertainment technology: Netflix, Spotify, YouTube, Twitch, gaming platforms

**E-commerce:**

- Marketplaces: Amazon, eBay, Alibaba, Etsy, Shopify

- Retail: Walmart, Target, Best Buy, Costco, Home Depot

- Specialized e-commerce: Wayfair, Chewy, regional platforms

**Food and Beverage:**

- Fast food chains: McDonald's, Burger King, Wendy's, KFC, Taco Bell, Chipotle

- Coffee shops: Starbucks, Dunkin', Peet's Coffee

- Beverage brands: Coca-Cola, Pepsi, Red Bull, Monster

- Food delivery: DoorDash, Uber Eats, Grubhub, Instacart

- Restaurants: Major casual dining chains

**Automotive:**

- Traditional manufacturers: Toyota, Honda, Ford, BMW, Mercedes-Benz, Volkswagen

- Electric vehicles: Tesla, Rivian, Lucid, Polestar, BYD

- Ride-sharing: Uber, Lyft

**Selection criteria:**

- Market leaders by revenue or market share

- Brands with significant consumer recognition

- Mix of legacy and emerging brands

- Brands likely to appear in recommendation/comparison queries

**Expected universe:** 100-150 brands total, distributed across industries

#### 2.2 Brand Mention Extraction

Develop a systematic approach to identify and extract brand mentions from assistant responses.

**Technical approach:**

- Use regular expressions with word boundary matching to avoid partial matches

- Case-insensitive matching to catch all variations

- Handle brand name variants (e.g., "McDonald's" vs "McDonalds")

- Record exact position in text (character offset)

- Extract surrounding context (50 characters before and after mention)

- Identify mention rank within response (1st mention, 2nd mention, etc.)

**Handling edge cases:**

- Common words that are also brands (e.g., "Amazon" river vs Amazon company)

- Brand aliases and abbreviations (e.g., "AWS" for Amazon Web Services)

- Multi-word brand names (e.g., "Burger King", "Red Bull")

**Validation:**

- Manual review of sample mentions to verify accuracy

- Calculate precision and recall on test set

- Adjust patterns based on false positives/negatives

#### 2.3 Brand Mention Dataset Creation

Transform extracted mentions into a structured dataset suitable for analysis.

**Dataset structure:**

Each row represents a single brand mention with attributes:

- Conversation identifier (linkable back to original conversation)

- Model used (gpt-3.5-turbo, gpt-4, etc.)

- Timestamp of conversation

- User's original query

- Assistant's full response

- Brand name

- Position in text (character offset)

- Rank in response (1st, 2nd, 3rd, etc.)

- Context snippet (surrounding text)

- Industry category

**Data quality checks:**

- Verify no duplicate mentions within same response

- Ensure rank ordering is correct

- Check for null or malformed entries

- Validate industry assignments

**Expected output:** Dataset with 10,000-50,000 brand mention records, depending on brand density in conversations.

#### 2.4 Coverage Analysis

Assess which brands from our universe actually appear in the dataset and identify gaps.

**Metrics:**

- Mention frequency distribution across brands

- Industry representation (are all industries well-represented?)

- Brands with zero mentions (candidates for removal from universe)

- Long-tail analysis (how many brands account for 80% of mentions?)

**Decision point:** If certain industries or brand categories are underrepresented, we may need to expand our sample or adjust our brand universe.

#### 2.5 Supplementary Data: Company Metadata

Enrich our brand dataset with company-level attributes that could influence LLM visibility.

**Data sources:**

- **Kaggle Crunchbase dataset:** Company founding dates, funding totals, employee counts, headquarters location, industry classifications

- **Alternative sources:** Fortune 1000 lists, company market capitalization datasets, brand valuation rankings

**Key attributes to collect:**

- Founded year (proxy for brand age/establishment)

- Total funding or revenue (proxy for size)

- Employee count (proxy for scale)

- Industry sector (for within-industry comparisons)

- Headquarters location (geographic influence)

- Company description (for qualitative context)

**Data preparation:**

- Standardize company names to match our brand universe

- Handle missing values (multiple imputation or flagging)

- Ensure dataset exceeds 50MB requirement through comprehensive coverage

#### 2.6 Supplementary Data: Wikipedia Pageviews

Use Wikipedia pageview counts as a proxy for public awareness and interest in brands.

**Methodology:**

- Access BigQuery's public Wikipedia pageviews dataset

- Query pageviews for brand-relevant Wikipedia pages

- Aggregate views over recent period (e.g., January 2024)

- Calculate total views and average daily views

**Mapping challenges:**

- Wikipedia page titles may differ from brand names (e.g., "Apple Inc." vs "Apple")

- Create mapping dictionary for accurate matching

- Handle disambiguation pages

- Some brands may not have dedicated Wikipedia pages

**Expected metrics:**

- Total pageviews (absolute interest)

- Average daily pageviews (sustained interest)

- Pageview trends if temporal data available

#### 2.7 Supplementary Data: Google Trends

Capture search interest over time using Google Trends as another awareness proxy.

**Methodology:**

- Use pytrends library to query Google Trends API

- Collect interest scores for top-mentioned brands

- Focus on past 12 months of data

- Normalize scores (Google Trends returns 0-100 relative interest)

**Limitations and handling:**

- API rate limits: restrict to top 20-30 brands

- Generic brand names may have ambiguous search intent

- Document any failed queries or ambiguous results

**Expected metrics:**

- Average interest score over 12 months

- Interest trend direction (increasing/decreasing)

#### 2.8 Data Integration and Upload

Combine all supplementary data sources into a comprehensive company dataset.

**Integration steps:**

- Join Wikipedia, Google Trends, and company metadata by brand name

- Resolve naming inconsistencies through manual mapping

- Create final enriched company table

- Verify data quality and completeness

**BigQuery upload:**

- Upload brand_mentions table (main analytical dataset)

- Upload companies table (supplementary enrichment data, satisfies 50MB requirement)

- Verify successful uploads and row counts

- Document table schemas

### Expected Deliverables

- Brand mentions dataset: 10,000-50,000 records with positional and contextual data

- Companies dataset: 100-150 brands with multi-source enrichment

- Coverage analysis showing brand mention distribution

- Data quality report documenting extraction accuracy and completeness

### Writeup Components

- Brand universe rationale and selection criteria

- Extraction methodology and validation results

- Coverage statistics: which brands appear, which don't

- Supplementary data sources and integration approach

- Data limitations and quality considerations

---

## Section 3: SQL Analysis and Data Visualization

### Objectives

Conduct comprehensive SQL-based analysis to answer our core research questions, and create visualizations that reveal patterns in brand visibility, positioning, sentiment, and competitive dynamics.

### Approach

#### 3.1 Query 1: Brand Mention Frequency Analysis

Identify which brands achieve the highest visibility in LLM responses.

**SQL approach:**

- Aggregate brand mentions across all conversations

- Group by brand and industry

- Calculate mention count, unique conversations with mention, average position, and first-position count

- Order by mention frequency to identify top performers

**Analytical focus:**

- Top 20 brands overall

- Top brands by industry

- Distribution of mentions (identify power law or long-tail patterns)

- Concentration metrics: what percentage of mentions do top 10 brands capture?

**Visualization 1: Top Brands Bar Chart**

- Horizontal bar chart showing top 15 brands by mention count

- Color-coded by industry to reveal industry dominance

- Include both absolute counts and percentage of total mentions

**Visualization 2: Industry Distribution Pie Chart**

- Show proportion of brand mentions by industry

- Assess whether industry representation matches expectations

**Key insights to extract:**

- Which brands dominate LLM responses?

- Is there industry bias in brand mentions?

- How concentrated is brand visibility (winner-takes-all vs distributed)?

#### 3.2 Query 2: Position Analysis (Core GEO Metric)

Analyze where brands appear in LLM responses, focusing on the critical "position 1" metric.

**SQL approach:**

- Group brand mentions by brand and rank_in_response

- Calculate count and percentage of mentions at each position

- Focus on positions 1-5 (most visible positions)

- Compute "position 1 win rate" for each brand

**Analytical focus:**

- Which brands consistently achieve position 1?

- Position distribution patterns (do some brands always appear first, others always second?)

- Industry differences in positioning

- Relationship between total mentions and position 1 rate

**Visualization 3: Position Heatmap**

- Rows: Top 10 brands

- Columns: Position in response (1, 2, 3, 4, 5)

- Cell values: Count of mentions at that position

- Color intensity: Darker = more mentions

- Reveals position patterns at a glance

**Derived metric: Position 1 Win Rate**

- Calculate: (mentions at position 1) / (total mentions) × 100%

- Rank brands by this critical GEO metric

- Compare win rates across industries

**Key insights to extract:**

- Who wins position 1 most consistently?

- Does high mention frequency guarantee position 1?

- Are there brands that "specialize" in certain positions?

#### 3.3 Query 3: Query Intent Classification and Impact

Analyze how different user query intents affect brand mention patterns.

**Intent classification approach:**

Develop rule-based classifier to categorize user queries:

- **Recommendation:** "recommend", "suggestion", "should I", "what to"

- **Comparison:** "vs", "versus", "compare", "better", "difference between"

- **Ranking:** "best", "top", "leading", "favorite", "most popular"

- **Informational:** "how to", "what is", "explain", "tell me about"

- **Review/Opinion:** "review", "opinion", "think about", "worth it"

- **Other:** Queries not matching above patterns

**SQL approach:**

- Apply classification to user_query field

- Group by query_intent and industry

- Calculate mention counts, unique brands, and average position by intent

- Identify which intents generate most brand mentions

**Analytical focus:**

- Which intent types trigger brand mentions most frequently?

- Does intent affect position (e.g., do "best" queries put brands in position 1 more often)?

- Industry-specific intent patterns

- Which intents favor certain brands?

**Visualization 4: Query Intent × Industry Heatmap**

- Rows: Industries (tech, e-commerce, food/beverage, automotive)

- Columns: Query intents (recommendation, comparison, ranking, etc.)

- Cell values: Number of brand mentions

- Reveals which intent-industry combinations are most common

**Key insights to extract:**

- What intent types are most valuable for brand visibility?

- Do different industries respond to different intents?

- Are there underserved intent-industry combinations?

#### 3.4 Query 4: Competitive Co-Mention Network

Identify which brands appear together in responses, revealing competitive clusters.

**SQL approach:**

- Self-join brand_mentions table on conversation_id

- Filter to pairs where brand_1 < brand_2 (avoid duplicates and self-pairs)

- Count co-occurrences of each brand pair

- Apply minimum threshold (e.g., 5 co-mentions) to focus on meaningful patterns

- Order by co-mention frequency

**Analytical focus:**

- Which brands are frequently mentioned together?

- Are co-mentions primarily within-industry or cross-industry?

- Identify competitive clusters (groups of brands that compete for mentions)

- Find surprising co-mention patterns

**Network metrics to calculate:**

- Co-mention frequency (edge weight)

- Total mentions per brand (node size)

- Industry clustering coefficient

**Visualization 5: Brand Co-Mention Network Graph**

- Nodes: Brands (size proportional to mention frequency)

- Edges: Co-mentions (thickness proportional to frequency)

- Colors: Industry categories

- Layout: Spring layout or force-directed graph

- Focus on top 50 brand pairs for readability

**Key insights to extract:**

- Who are the primary competitors in LLM responses?

- Are there clear competitive clusters?

- Do certain brands "bridge" between industries?

- What does co-mention frequency reveal about substitutability?

#### 3.5 Query 5: Sentiment and Context Analysis

Analyze how brands are described in LLM responses using sentiment analysis.

**Sentiment analysis approach:**

- Use TextBlob library to analyze sentiment of context snippets

- Calculate polarity score (-1 to +1, negative to positive)

- Classify as positive (> 0.1), neutral (-0.1 to 0.1), or negative (< -0.1)

- Apply to all brand mention contexts

**SQL approach:**

- Group by brand, industry, and sentiment

- Calculate counts and average sentiment scores

- Compute sentiment ratio: positive mentions / negative mentions

- Analyze correlation between sentiment and position

**Analytical focus:**

- Which brands receive most positive mentions?

- Industry differences in sentiment

- Does sentiment correlate with position in response?

- Sentiment distribution: mostly neutral, polarized, or positive-skewed?

**Visualization 6: Sentiment Stacked Bar Chart**

- Rows: Top 20 brands

- Stacks: Positive, neutral, negative mention counts

- Shows absolute sentiment distribution per brand

**Visualization 7: Sentiment Ratio Analysis**

- Bar chart of sentiment ratio (positive/negative) for top 15 brands

- Horizontal line at 1.0 (equal positive and negative)

- Identifies brands with particularly positive or negative contexts

**Key insights to extract:**

- Do higher-mentioned brands have more positive sentiment?

- Are there brands with high visibility but negative sentiment?

- Does sentiment quality predict position?

#### 3.6 Query 6: Query Pattern Optimization (Critical for GEO)

Analyze specific query phrasings to identify patterns that maximize brand visibility.

**Query pattern classification approach:**

Develop fine-grained classifier for specific phrasings:

- **"best_x":** "best", "top", "greatest", "finest"

- **"recommend_x":** "recommend", "suggest", "advice on"

- **"x_vs_y":** " vs ", " versus ", "or", "compared to"

- **"should_i":** "should i", "worth it", "is it good"

- **"alternatives":** "alternative", "instead of", "replacement for"

- **"reviews":** "review", "opinion on", "experience with"

- **"what_is":** "what is", "tell me about", "explain"

- **"how_to":** "how to", "how do i", "guide to"

- **"other":** Other patterns

**SQL approach:**

- Apply pattern classification to user queries

- Group by query_pattern and industry

- Calculate mention counts, unique brands, average position, and position 1 rate

- Identify which patterns most effectively trigger brand mentions

- Compute position 1 rate by pattern (critical GEO insight)

**Analytical focus:**

- Which query patterns generate most brand mentions?

- Which patterns result in highest position 1 rates?

- Industry-specific pattern effectiveness

- Trade-offs between mention volume and position quality

**Visualization 8: Query Pattern Performance**

- Dual bar chart:

  - Chart 1: Total mentions by query pattern

  - Chart 2: Position 1 win rate by query pattern

- Identifies patterns that are both high-volume and high-quality

**Key insights to extract (most actionable for GEO):**

- What phrasings maximize position 1 placement?

- Are "best" queries better than "recommend" queries?

- Which patterns favor which industries?

- Underutilized patterns with high position 1 rates

### Additional Analysis Queries

#### 3.7 Temporal Trends (if sufficient date range)

Analyze how brand mentions change over time.

**SQL approach:**

- Extract date from timestamp

- Group by date and brand

- Calculate daily mention counts

- Identify trending brands (increasing mentions over time)

**Visualization:** Time series plots showing mention trends for top brands

#### 3.8 Response Length and Brand Mentions

Investigate whether longer responses mention more brands.

**SQL approach:**

- Calculate response length (character count)

- Group by length buckets

- Analyze average brands mentioned per length category

### Expected Deliverables

- Six comprehensive SQL queries answering core research questions

- Eight visualizations revealing patterns in brand visibility

- Statistical summaries for each analysis

- Identification of actionable GEO insights from each query

### Writeup Components

- Detailed findings for each of the six queries

- Interpretation of visualizations

- Cross-cutting insights that emerge from multiple analyses

- Preliminary GEO recommendations based on query findings

---

## Section 4: Machine Learning - Position Prediction Model

### Objectives

Build a predictive model that forecasts which position (1st, 2nd, 3rd, 4+) a brand will occupy in an LLM response based on brand characteristics, query attributes, and contextual factors. This model will reveal which features are most predictive of favorable positioning.

### Approach

#### 4.1 Feature Engineering

Transform raw data into predictive features suitable for machine learning.

**Query-level features:**

- **Query length:** Character count of user query

- **Query word count:** Number of words in query

- **Query complexity:** Presence of question marks, multiple sentences

- **Query intent:** Encoded categorical variable (recommendation, comparison, ranking, etc.)

- **Query pattern:** Encoded categorical variable (best_x, recommend_x, x_vs_y, etc.)

**Brand-level features:**

- **Historical mention frequency:** Total mentions in dataset (popularity proxy)

- **Historical average position:** Brand's average position across all mentions

- **Industry category:** Encoded categorical variable

- **Wikipedia pageviews:** Total and average daily views (awareness proxy)

- **Google Trends score:** Search interest (if available)

- **Company age:** Years since founding (establishment proxy)

- **Company size:** Revenue or employee count (scale proxy)

**Context-level features:**

- **Sentiment score:** Polarity of mention context (-1 to +1)

- **Sentiment category:** Encoded positive/neutral/negative

- **Context length:** Number of characters in extracted context

- **Response length:** Total length of assistant response

**Competitive features:**

- **Number of competing brands:** Count of other brands in same response

- **Co-mention density:** Ratio of brands to response length

**Temporal features (if applicable):**

- **Day of week:** User behavior patterns

- **Hour of day:** Time-of-day effects

**Feature encoding:**

- Apply label encoding to categorical variables (industry, intent, pattern, sentiment)

- Standardize or normalize continuous variables if using distance-based algorithms

- Handle missing values through imputation or indicator variables

**Expected feature set:** 12-20 features after encoding

#### 4.2 Target Variable Definition

Define the prediction target to balance model complexity and business relevance.

**Target variable: position_class**

- **1:** Brand appears in position 1 (first mention)

- **2:** Brand appears in position 2 (second mention)

- **3:** Brand appears in position 3 (third mention)

- **4:** Brand appears in position 4 or later (grouped due to lower frequency)

**Rationale:**

- Multiclass classification with 4 classes

- Position 1 is most valuable (highest visibility)

- Positions 2-3 still highly visible

- Position 4+ grouped due to diminishing importance and sample size

**Alternative targets considered:**

- Binary: Position 1 vs not (simpler but loses information)

- Regression: Exact position (but positions are ordinal, not truly continuous)

#### 4.3 Train-Test Split

Divide data to enable model evaluation on unseen data.

**Split strategy:**

- 80% training, 20% testing

- Stratified split to maintain class balance

- Random seed for reproducibility

**Data preparation:**

- Remove records with missing values in critical features

- Verify sufficient samples in each class (minimum 100 records per class)

- Document final dataset sizes

**Expected sizes:**

- Training set: ~8,000-40,000 records (depending on total brand mentions)

- Test set: ~2,000-10,000 records

#### 4.4 Model Selection and Training

Use BigQuery ML for logistic regression model.

**Model choice: Logistic Regression**

- **Rationale:**

  - Interpretable coefficients (critical for understanding drivers)

  - Handles multiclass classification (softmax/multinomial logistic regression)

  - Computationally efficient

  - Works well with categorical and continuous features

  - Required for CS145 project

**BigQuery ML implementation:**

- Create model using CREATE MODEL statement

- Specify model_type='LOGISTIC_REG'

- Set input_label_cols=['position_class']

- Apply auto_class_weights=TRUE to handle any class imbalance

- Train on filtered dataset (split='train')

**Training process:**

- BigQuery ML automatically handles:

  - Feature normalization

  - Model fitting

  - Hyperparameter tuning (learning rate, iterations)

  - Convergence checking

**Expected training time:** 2-5 minutes depending on dataset size

#### 4.5 Model Evaluation

Assess model performance using appropriate classification metrics.

**Evaluation metrics:**

- **Accuracy:** Overall correctness (baseline metric)

- **Precision:** Per-class precision (correctness of predictions for each position)

- **Recall:** Per-class recall (coverage of actual instances in each position)

- **F1 Score:** Harmonic mean of precision and recall

- **Confusion Matrix:** Detailed view of prediction patterns

**Evaluation approach:**

- Use BigQuery ML's ML.EVALUATE function on test set

- Generate confusion matrix using ML.PREDICT and aggregation

- Calculate per-class metrics to identify position-specific performance

**Performance expectations:**

- Baseline accuracy (random guessing): ~25% (4 classes)

- Target accuracy: >40% (significantly better than random)

- Position 1 likely easier to predict (clearer signal) than positions 2-4

**Confusion matrix analysis:**

- Are predictions systematically biased (e.g., over-predicting position 1)?

- Are adjacent positions confused (e.g., predicting 2 when actual is 3)?

- Which classes are hardest to predict?

#### 4.6 Feature Importance Analysis

Identify which features most strongly predict brand position.

**Approach:**

- Extract feature weights from trained model using ML.WEIGHTS

- Analyze coefficient magnitudes (absolute values indicate importance)

- Interpret coefficient signs (positive increases position number, negative decreases)

**Key questions:**

- Which query features matter most (intent, pattern, length)?

- Do brand characteristics (size, age, awareness) predict position?

- Is sentiment predictive of position?

- Are competitive features (number of competitors) important?

**Visualization: Feature Importance Bar Chart**

- Top 15 features by absolute weight

- Color-coded by sign (positive vs negative effect)

- Interpret in terms of GEO strategy

**Expected insights:**

- Historical performance (brand_historical_avg_position) likely highly predictive

- Query intent/pattern probably strong predictors

- Sentiment may have moderate effect

- Company size may have weaker effect than expected

#### 4.7 Model Interpretation and Insights

Translate model findings into business understanding.

**Coefficient interpretation:**

- Positive coefficients: Increase in feature increases position number (moves brand later in list)

- Negative coefficients: Increase in feature decreases position number (moves brand earlier in list)

**Example interpretations:**

- If query_intent_encoded has negative coefficient: Certain intents favor earlier positions

- If brand_mention_frequency has negative coefficient: Popular brands tend to appear first

- If sentiment_score has negative coefficient: Positive sentiment correlates with earlier position

**GEO implications:**

- Features with large negative coefficients are levers for achieving position 1

- Controllable features (query patterns) vs fixed features (brand size)

- Industry-specific strategies based on industry coefficient

#### 4.8 Prediction Scenarios

Use trained model to predict outcomes for hypothetical scenarios.

**Scenario design:**

- Create synthetic feature combinations representing:

  - Established brand + recommendation query + positive sentiment

  - Emerging brand + comparison query + neutral sentiment

  - Large brand + informational query + low competition

- Compare predicted positions across scenarios

**Analysis:**

- Which scenarios most reliably achieve position 1?

- Sensitivity analysis: How much does changing one feature affect prediction?

- Identify optimal feature combinations for GEO

### Expected Deliverables

- Trained logistic regression model in BigQuery

- Comprehensive evaluation metrics (accuracy, precision, recall, F1)

- Confusion matrix visualization

- Feature importance analysis with top 15 features

- Model interpretation and GEO insights

- Prediction scenarios demonstrating model application

### Writeup Components

- Feature engineering rationale and approach

- Model selection justification

- Performance metrics and interpretation

- Feature importance findings

- Key predictive factors for brand positioning

- Actionable insights: which features can brands control to improve positioning?

- Model limitations and future improvements

---

## Section 5: Query Performance Analysis

### Objectives

Analyze the computational performance of our SQL queries, calculate I/O costs using join algorithms from CS145 lectures, and provide optimization recommendations for scaling to 10x and 100x dataset sizes.

### Approach

#### 5.1 Query Execution Statistics Collection

Systematically document the performance characteristics of our key queries.

**Queries to analyze:**

- **Query 1:** Co-mention network (self-join with aggregation)

- **Query 2:** Multi-way join (brand_mentions + companies with multiple groupings)

**Metrics to collect for each query:**

- **Bytes processed:** Total data scanned by BigQuery

- **Bytes billed:** Data charged (may differ from processed due to caching)

- **Slot milliseconds:** Computational resources consumed

- **Duration:** Wall-clock execution time

- **Rows processed:** Input data volume

- **Rows returned:** Output data volume

**Collection method:**

- Execute queries through BigQuery client

- Access job statistics through client.query() job object

- Record ended - started for duration

- Document all metrics in structured format

**Cost calculation:**

- BigQuery pricing: $5 per TB processed

- Calculate cost per query: (bytes_processed / 1024^4) × 5

- Project monthly costs assuming typical query volumes

#### 5.2 Query Plan Analysis

Examine how BigQuery executes our queries to identify bottlenecks.

**Query plan components:**

- **Stages:** Sequential execution steps

- **Stage details:** Records read, records written, computation time

- **Shuffle operations:** Data movement between stages

- **Partition pruning:** Whether partitioning helps (if applicable)

**Analysis focus:**

- Which stages consume most time?

- Where does data shuffling occur?

- Are there opportunities for filter pushdown?

- How does BigQuery choose join order?

**For self-join query (co-mentions):**

- Identify cartesian product stage (most expensive)

- Measure cost of grouping and aggregation

- Assess impact of HAVING clause filtering

**For multi-way join:**

- Determine join order chosen by BigQuery

- Assess whether smaller table (companies) is used as build side

- Identify any broadcast joins vs shuffle joins

#### 5.3 Optimization Experiment: Column Selection

Demonstrate impact of selecting specific columns vs SELECT *.

**Experiment design:**

- **Query A (baseline):** SELECT * with WHERE clause

- **Query B (optimized):** SELECT specific columns with same WHERE clause

- Ensure identical result sets for fair comparison

**Metrics comparison:**

- Bytes processed (should differ significantly)

- Execution time (may differ less than bytes processed)

- Cost difference

**Expected results:**

- Selecting specific columns should reduce bytes processed by 40-80%

- Demonstrates importance of column pruning in columnar storage

**GEO relevance:**

- Cost optimization matters when running queries frequently

- Production systems should always specify needed columns

#### 5.4 Theoretical Join Cost Analysis

Apply CS145 join algorithms to calculate I/O costs for current and scaled datasets.

**Dataset size estimation:**

- **Current scale:**

  - brand_mentions table: R rows, estimate P(R) pages

  - companies table: S rows, estimate P(S) pages

  - Assume page size: 8KB

  - Estimate pages from bytes_processed statistics

**Assumptions:**

- Buffer size B: 100 pages (realistic for BigQuery slot)

- Join on conversation_id (for self-join) or brand/company_name

- No indexes (BigQuery doesn't use traditional indexes)

**Algorithm 1: Block Nested Loop Join (BNLJ)**

- Cost formula: P(R) + (P(R) × P(S)) / (B - 1)

- Calculate for brand_mentions self-join

- Calculate for brand_mentions + companies join

**Algorithm 2: Sort-Merge Join (SMJ)**

- Cost formula: 3 × (P(R) + P(S)) for two-pass external sort

- Assumes both relations sorted, then merged

- More efficient for large tables

**Algorithm 3: Hash-Partition Join (HPJ)**

- Cost formula: 3 × (P(R) + P(S)) if single partitioning suffices

- Assumes hash table fits in memory for smaller relation

- Similar cost to SMJ in I/O terms

**Current scale calculations:**

- Plug in estimated P(R) and P(S) values

- Calculate cost for each algorithm

- Identify which algorithm BigQuery likely uses

#### 5.5 Scaling Projections: 10x Dataset

Project costs and optimal algorithms for dataset 10x larger.

**Scenario:**

- brand_mentions: 10x rows (2M mentions instead of 200K)

- companies: same size (brand universe doesn't grow proportionally)

- P(R) scales 10x, P(S) remains constant

**Recalculate costs:**

- BNLJ: P(R) + (P(R) × P(S)) / (B - 1)

  - Dominated by P(R) × P(S) term

  - Grows linearly with P(R)

- SMJ: 3 × (P(R) + P(S))

  - Grows linearly with P(R)

  - More favorable ratio compared to BNLJ at scale

- HPJ: 3 × (P(R) + P(S))

  - Similar to SMJ

**Cost comparison:**

- Calculate ratio: BNLJ_cost / SMJ_cost at 10x scale

- Likely SMJ becomes favorable if not already

**BigQuery cost projections:**

- Bytes processed scales linearly (10x)

- Query cost: ~10x increase

- Monthly costs if running queries frequently

**Optimization recommendations for 10x:**

- Implement table partitioning by timestamp or industry

- Consider clustering by frequently-joined columns

- Use materialized views for common aggregations

- Monitor query costs and set budgets

#### 5.6 Scaling Projections: 100x Dataset

Project costs and optimal algorithms for dataset 100x larger.

**Scenario:**

- brand_mentions: 100x rows (20M mentions)

- P(R) scales 100x

**Recalculate costs:**

- BNLJ: Now completely impractical

  - P(R) × P(S) term dominates

  - 100x increase in P(R) = 100x increase in cost

- SMJ: Still linear scaling

  - 3 × (100 × P(R) + P(S))

  - Only ~100x increase

- HPJ: Similar to SMJ

**Cost comparison:**

- BNLJ likely 10-100x more expensive than SMJ at this scale

- SMJ or HPJ clearly superior

**BigQuery cost projections:**

- Bytes processed: ~100x current

- Per-query cost: $X (could be significant)

- Annual costs: Potentially thousands of dollars for frequent queries

**Optimization recommendations for 100x:**

- Partitioning is essential

  - Partition by date (monthly or daily)

  - Enables pruning of 95%+ data for time-range queries

- Clustering by brand and conversation_id

  - Co-locates related rows

  - Reduces data scanning for joins

- Materialized views for common aggregations

  - Pre-compute expensive operations

  - Refresh on schedule

- Consider approximate queries

  - Use sampling for exploratory analysis

  - Full queries only for production reports

#### 5.7 Optimization Strategy Summary

Synthesize findings into actionable recommendations.

**Immediate optimizations (current scale):**

- Always specify exact columns needed (avoid SELECT *)

- Use WHERE clauses to filter early

- Leverage BigQuery's 24-hour result caching

- Structure queries to enable predicate pushdown

**10x scale preparations:**

- Implement timestamp partitioning (daily or monthly)

- Add clustering on frequently-joined keys

- Create materialized views for dashboards

- Monitor query costs and set budgets

**100x scale requirements:**

- Partitioning is mandatory for acceptable performance

- Multi-level partitioning/clustering strategy

- Dedicated data warehouse optimization team

- Consider switching from BNLJ to SMJ explicitly (if controllable)

- Implement tiered storage (hot vs cold data)

**Cost-benefit analysis:**

- Partitioning setup cost: Hours of engineering time

- Ongoing savings: 70-90% reduction in bytes processed

- Clustering setup cost: Additional engineering hours

- Ongoing savings: 40-60% reduction on join queries

- ROI: Positive after ~100-1000 queries depending on dataset size

### Expected Deliverables

- Query execution statistics for 2 complex queries

- Column selection optimization experiment results

- Theoretical join cost calculations for current, 10x, and 100x scales

- Algorithm comparison tables showing when SMJ becomes superior

- Cost projections for scaled datasets

- Comprehensive optimization recommendations

### Writeup Components

- Query performance baseline metrics

- Optimization experiment findings

- Detailed join algorithm cost analysis with calculations

- Scaling projections and trade-offs

- Recommended optimization strategy by scale tier

- Cost-benefit analysis of optimization investments

---

## Section 6: Conclusions and GEO Recommendations

### Objectives

Synthesize findings from all analyses to provide actionable GEO strategies, acknowledge limitations, and propose future research directions.

### Approach

#### 6.1 Key Findings Synthesis

Summarize the most important discoveries from each analytical section.

**From Position Analysis:**

- Which brands dominate position 1 and why

- Whether large brands always win or if other factors matter

- Position concentration vs distribution patterns

**From Sentiment Analysis:**

- Relationship between sentiment and position

- Brands with notably positive or negative contexts

- Whether sentiment quality predicts visibility

**From Competitive Analysis:**

- Identification of clear competitive clusters

- Cross-industry competition patterns

- Strategic implications of co-mention networks

**From Query Pattern Analysis:**

- Most effective query patterns for brand visibility

- Intent-specific strategies

- Actionable phrasings for optimization

**From ML Model:**

- Top predictive features for position

- Controllable vs fixed factors

- Surprising findings about what drives position 1

**From Performance Analysis:**

- Scalability considerations

- Cost implications of GEO monitoring at scale

- Optimization priorities

#### 6.2 Industry-Specific GEO Recommendations

Develop tailored strategies for each industry based on findings.

**Technology Brands:**

- Optimal query patterns identified from analysis

- Positioning strategies relative to competitors

- Sentiment management recommendations

- Specific action items based on data

**E-commerce Brands:**

- Query intent targeting based on e-commerce mention patterns

- Competitive positioning relative to marketplace dominance

- Recommendations for smaller players vs large platforms

**Food/Beverage Brands:**

- Local vs national chain implications

- Review/opinion query optimization

- Comparison query strategies

**Automotive Brands:**

- EV vs traditional manufacturer positioning

- Comparison query dominance strategies

- Handling multi-brand mentions

#### 6.3 Universal GEO Principles

Identify cross-cutting strategies applicable to all brands.

**Principle 1: Position 1 as Primary KPI**

- Rationale from analysis showing position 1 importance

- Measurement approach

- Optimization tactics

**Principle 2: Query Pattern Optimization**

- Specific phrasings that maximize visibility

- Content structuring recommendations

- A/B testing framework for query patterns

**Principle 3: Sentiment Quality Management**

- Ensuring positive context in brand mentions

- Addressing negative sentiment patterns

- Building positive association patterns

**Principle 4: Competitive Awareness**

- Monitoring co-mention patterns

- Strategic positioning relative to competitors

- Defensive and offensive co-mention strategies

**Principle 5: Continuous Measurement**

- Establishing baseline metrics

- Regular monitoring cadence

- Key metrics to track over time

#### 6.4 Limitations and Constraints

Honestly assess the boundaries of our analysis.

**Data Limitations:**

- Single LLM family (ChatGPT only)

- Temporal coverage constraints

- English-language only

- Limited brand universe

- Sampling effects

**Methodological Limitations:**

- Correlation vs causation

- Brand mention extraction accuracy

- Sentiment analysis limitations

- Model generalization concerns

**Practical Limitations:**

- Dynamic nature of LLMs (models update frequently)

- Training data cutoffs affecting recency

- Platform-specific behaviors

- User population bias in LMSYS dataset

**Acknowledging what we cannot conclude:**

- Causation of brand positioning

- Effectiveness of GEO interventions (no A/B test)

- Generalization to other LLMs

- Long-term trends

#### 6.5 Future Research Directions

Propose extensions and improvements to this analysis.

**Immediate Extensions:**

- Multi-model comparison (GPT vs Claude vs Gemini)

- Temporal analysis with longer time series

- Expanded brand universe (more industries, long-tail brands)

- Deeper sentiment analysis using advanced NLP

**Advanced Research Questions:**

- Causal inference: Do certain content structures cause better positioning?

- Intervention studies: Can brands experimentally improve visibility?

- User satisfaction: Does brand positioning correlate with user ratings?

- Bias analysis: Are there systematic biases in brand representation?

**Production Applications:**

- Real-time GEO monitoring dashboard

- Automated alerting for visibility changes

- Competitive intelligence platform

- A/B testing framework for brand content

**Methodological Improvements:**

- More sophisticated NLP for brand detection

- Deep learning models for position prediction

- Graph neural networks for competitive analysis

- Time series forecasting for trend prediction

#### 6.6 Actionable Implementation Roadmap

Provide step-by-step guide for brands to implement GEO strategies.

**Phase 1: Baseline Measurement (Month 1)**

- Establish current brand visibility metrics

- Measure position 1 rate, mention frequency, sentiment

- Identify top competitors and co-mention patterns

- Document current query patterns triggering mentions

**Phase 2: Quick Wins (Months 2-3)**

- Optimize content for high-performing query patterns

- Target underutilized intents with high position 1 rates

- Improve sentiment in existing brand contexts

- Address negative sentiment patterns

**Phase 3: Strategic Positioning (Months 4-6)**

- Develop competitive positioning strategy

- Create content targeting specific intent-pattern combinations

- A/B test different content approaches

- Build relationships to improve brand associations

**Phase 4: Continuous Optimization (Ongoing)**

- Monthly monitoring of key metrics

- Quarterly competitive analysis

- Regular content updates based on performance

- Adapt to LLM updates and changes

#### 6.7 Impact Assessment Framework

Propose methods to measure GEO effectiveness over time.

**Key Performance Indicators:**

- Position 1 rate (primary metric)

- Total mention frequency

- Positive sentiment ratio

- Co-mention with preferred brands

- Query pattern coverage

**Measurement Approach:**

- Baseline metrics from this analysis

- Quarterly re-measurement using same methodology

- Year-over-year comparisons

- Competitive benchmarking

**Expected Impact Magnitudes:**

- Realistic improvement targets

- Timeline for observing changes

- Investment vs return considerations

### Expected Deliverables

- Comprehensive findings summary

- Industry-specific GEO strategy documents

- Universal GEO principles guide

- Honest limitations assessment

- Future research agenda

- Implementation roadmap for practitioners

### Writeup Components

- Executive summary of key findings

- Detailed GEO recommendations by industry

- Universal principles applicable to all brands

- Frank discussion of limitations and constraints

- Exciting future research directions

- Practical implementation guide

- Measurement framework for ongoing optimization

---

## Appendix: Technical Documentation

### Data Dictionary

Comprehensive documentation of all tables, columns, data types, and definitions to ensure reproducibility and clarity.

### Code Repository Structure

Organization of analysis code for maintainability and sharing.

### Reproducibility Notes

Instructions for replicating the analysis, including:

- Environment setup requirements

- Data download procedures

- BigQuery configuration

- Sampling seeds and parameters

- Library versions

### Acknowledgments

Recognition of data sources, tools, and resources used in the project.

---

## Project Success Criteria

This project will be considered successful if it:

1. **Answers Core Research Questions:** Provides data-driven insights into brand positioning, sentiment, competitive dynamics, and query optimization in LLM responses.

2. **Delivers Actionable GEO Recommendations:** Offers specific, evidence-based strategies that brands could implement to improve visibility in LLM outputs.

3. **Demonstrates Technical Proficiency:** Shows competent use of SQL, BigQuery, machine learning, and data visualization to analyze large-scale conversational data.

4. **Achieves Statistical Rigor:** Employs appropriate analytical methods, acknowledges limitations, and avoids overgeneralizing from findings.

5. **Provides Practical Value:** Creates a reusable framework for GEO analysis that could be applied to other LLMs, industries, or time periods.

6. **Communicates Clearly:** Presents findings in accessible language with effective visualizations that tell a compelling data story.

