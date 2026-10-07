---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - ai
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
  - Research & Concepts
---
## Worked example

**Scenario:** Creating a Data Analysis Report
**Business context:** A marketing team needs to analyze customer survey data to improve their product
strategy.
We'll show how to evolve a prompt from basic to sophisticated using both frameworks.
## Level 1: Basic Prompt (Beginner)
### Basic Application

**Task:** Analyze this data

**Context:** Marketing survey
Analyze this customer survey data and tell me what it means.
[Survey data attached]
Problems with this approach:

- The task is vague.
- The business goal is missing.
- There is no example to learn from.
- The required output format and tone are unspecified.
## Level 2: Structured Prompt (Intermediate)
### Google AI Framework (TCREI)

**Task:** Create executive summary of customer satisfaction trends

**Context:** Survey results, Marketing team needs insights for Q4 strategy

**Reference:** Executive summary of a recent analysis

**Evaluate:** Think about what the output has to satisfy and evaluate accordingly

**Iterate:** Refine the output in subsequent passes.
You are a data analyst for our marketing team. Analyze the attached customer survey data to create an
executive summary similar to
 the example below. Focus on satisfaction trends and actionable insights for our Q4 product strategy.

<example analysis>
[Example omitted from the source note.]
</example analysis>

[Survey data attached]
## Level 3: Advanced Prompt (Expert)
### Combined framework implementation

This version applies Anthropic's “Uncle Charlie Takes Every Dog Past Fancy Parks” framework.

```text
<role>
You are a senior marketing data analyst with 10+ years of experience in customer behavior analysis and
 strategic recommendations.
</role>

<task_context>
Our product team
is preparing for Q4 strategy meetings and needs a comprehensive analysis of our latest customer satisfaction
survey. The goal is to identify specific areas for product improvement and customer retention strategies.
</task_context>

<tone_context>
Use a professional, data-driven tone appropriate for C-level executives. Be confident in your analysis but
acknowledge any limitations in the data.
</tone_context>

<detailed_task_description>
Analyze the customer survey data to produce an executive briefing that includes:
1. **Trend Analysis**: Compare current satisfaction scores against historical data
2. **Segmentation Insights**: Break down findings by customer demographics and
 usage patterns
3. **Priority Matrix**: Rank issues by impact and
 feasibility
4. **Strategic Recommendations**: Provide 3-5
 specific, actionable recommendations

Rules:
- Only make claims supported by the data
- If data is insufficient for
 a conclusion, explicitly state this
- Include confidence levels for
 your recommendations
- Highlight any surprising or counter-intuitive findings
</detailed_task_description>

<examples>
<example>
**Finding**: "Customer satisfaction with mobile app usability dropped 15% among users aged 55+ compared to Q2
(confidence:
high, n=247)"

**Recommendation**: "Implement simplified navigation mode for senior users by Q4, projected to improve
retention by 8-12% based on
similar initiatives in our 2023 web platform update"
</example>
</examples>

<thinking_instructions>
Before providing your analysis, please think through:
1. What patterns do you see in the data?
2. How do current results compare to historical trends?
3. What are the most statistically significant findings?
4. What might be driving the key changes?
5. Which recommendations would have the highest business impact?

Show your reasoning step-by-step before presenting conclusions.
</thinking_instructions>

<output_format>
Structure your response as:
## Executive Summary
[2-3 bullet points of critical insights]
## Detailed Analysis
### Satisfaction Trends
### Segment Breakdown
### Statistical Significance
## Strategic Recommendations
[Numbered list with impact/effort assessment]
## Appendix
[Supporting charts and detailed statistics]
</output_format>

Please analyze the provided survey data following these guidelines.

[Survey data attached]
```

## Level 4: Master-Level with Advanced Techniques
### Incorporating Tree-of-Thought, Chain-of-Thought, and Prompt Chaining
This version combines multiple expert roles, scenario testing, verification, and business constraints.

```text
<role>
You are a consulting team of three world-class experts collaborating on a high-stakes analysis:
- **Dr. Sarah Chen**: Senior Data Scientist
(PhD Statistics, 15+ years in customer analytics)
- **Marcus Rodriguez**: Behavioral Psychology Expert
(Specializes in consumer decision patterns)
- **Elena Petrov**: Strategic Business Consultant
(Former McKinsey Principal, product strategy specialist)
</role>

<task_context>
Critical Q4 strategy decision point: Our company's future product roadmap depends on this analysis. The CEO
will present findings to the board next week. This analysis will influence $10M+ in product development
investments and affect 2.5M+ customers.
</task_context>

<tone_context>
Board-level presentation quality. Authoritative yet nuanced. Acknowledge uncertainty where it exists but
provide confident recommendations where data supports them.
</tone_context>

<detailed_task_description>
**Multi-Stage Analysis Process:**

**Phase 1**: Statistical Foundation
(Dr. Chen leads)
- Data quality assessment and cleaning recommendations
- Statistical significance testing for all major findings
- Trend analysis with confidence intervals
- Correlation and causation analysis

**Phase 2**: Behavioral Interpretation
(Marcus leads)
- Customer psychology patterns identification
- Motivation and friction point analysis
- Segment-specific behavioral drivers
- Emotional satisfaction vs functional satisfaction breakdown

**Phase 3**: Strategic Translation
(Elena leads)
- Business impact quantification
- Competitive positioning implications
- Resource allocation recommendations
- Risk assessment for each recommendation

**Phase 4**: Synthesis & Conflict Resolution
(All three collaborate)
- Reconcile conflicting interpretations
- Develop unified recommendations
- Create implementation timeline
- Identify success metrics
</detailed_task_description>

<tree_of_thought_scenarios>
**Scenario Planning**: Before final analysis, consider these three interpretations:

**Scenario Alpha**
(Market Maturity)
:
- Customer satisfaction changes reflect natural product lifecycle maturation
- Customers becoming more sophisticated/demanding
- Competitive pressure increasing satisfaction thresholds

**Scenario Beta**
(Demographic Shift)
:
- Primary user base demographic evolution driving changes
- Generational preference shifts affecting satisfaction metrics
- New user acquisition changing overall satisfaction profile

**Scenario Gamma**
(Product-Market Misalignment)
:
- Fundamental disconnect between product direction and customer needs
- Feature development priorities misaligned with user value perception
- Market positioning not matching actual customer experience

*For each scenario, develop distinct strategic implications and test against the data.*
</tree_of_thought_scenarios>

<chain_of_thought_instructions>
**Required Thinking Process** (make the reasoning explicit):

1. **Data Quality Assessment**: "First, let me examine the data integrity, sample sizes, and potential
biases..."

2. **Pattern Recognition**: "Looking at the patterns, I notice... This suggests... However, I should also
consider..."

3. **Statistical Validation**: "Testing for significance, I find... The confidence level is... This means..."

4. **Behavioral Interpretation**: "From a psychology perspective, this pattern indicates... The underlying
motivation appears to be..."

5. **Business Translation**: "Strategically, this translates to... The business impact would be... The
implementation complexity is..."

6. **Scenario Testing**: "Under Scenario Alpha, this would mean... Under Scenario Beta... Under Scenario
Gamma..."

7. **Synthesis**: "Weighing all factors, the most likely explanation is... Therefore, I recommend..."
</chain_of_thought_instructions>

<verification_and_grounding>
**Mandatory Verification Steps**:
1. **Quote exact data points** to support every major claim
2. **Calculate and state confidence levels** for all recommendations
3. **Identify data limitations** that could affect conclusions
4. **Cross-reference with industry benchmarks** where possible
5. **Flag potential biases** in survey methodology or response patterns
6. **Test competing hypotheses** for major findings
</verification_and_grounding>

<examples>
<high_quality_example>
**Statistical Finding**: "Mobile app satisfaction decreased 18.3% (±3.2%) among users 55+ compared to Q2
(n=247, p<0.001, high confidence)
"

**Behavioral Interpretation**: "This likely reflects increased cognitive load from recent UI changes, as users
55+ show 40% higher abandonment rates on redesigned features
(per heatmap data)
"

**Strategic Recommendation**: "Priority 1: Implement optional 'Classic Mode' toggle for users 55+. Estimated
development cost: $85K. Projected retention improvement: 12-15% in this segment. ROI: 300% within 6 months
based on LTV calculations."
</high_quality_example>
</examples>

<constraints_and_reality_checks>
**Business Constraints**:
- Development budget cap: $500K per quarter
- Must launch before holiday season (8 weeks max)
- Engineering capacity: 2 senior developers available
- Cannot break existing user workflows
- Must maintain GDPR/privacy compliance
- iOS App Store review process: 2-week minimum

**Success Criteria**:
- Minimum 5% improvement in overall satisfaction scores
- No decrease in user engagement metrics
- Positive ROI within 2 quarters
- Implementation feasibility score >7/10 from engineering
</constraints_and_reality_checks>

<output_format>
## Executive Dashboard
**Critical Metrics**:
[Key numbers for immediate decision-making]
**Confidence Level**:
[Overall confidence in recommendations]
**Timeline**:
[Implementation timeline with milestones]

## Detailed Analysis

### Statistical Foundation
(Dr. Chen)
[Data quality, significance, trends with confidence intervals]

### Behavioral Insights
(Marcus)
[Psychology patterns, motivations, segment behaviors]

### Strategic Implications
(Elena)
[Business impact, competitive positioning, resource needs]

### Multi-Scenario Planning
**Most Likely Scenario**:
[Evidence-based primary interpretation]
**Alternative Scenarios**:
[Backup explanations with probability estimates]

## Unified Recommendations
1. **Immediate Actions**
(0-4 weeks)
2. **Short-term Initiatives**
(1-3 months)
3. **Strategic Investments**
(3-12 months)

*Each recommendation includes: Impact estimate, Effort required, Success metrics, Risk factors*

## Implementation Roadmap
[Month-by-month execution plan with dependencies and checkpoints]

## Appendix
### Supporting Data
### Methodology Notes
### Confidence Calculations
### Alternative Interpretations Considered
</output_format>

<prefilled_response>
Assistant: I'll conduct this analysis using our three-expert collaborative approach, working through each
phase systematically.

## Phase 1: Statistical Foundation
(Dr. Chen)

Let me begin by examining the data quality and establishing our analytical foundation...
</prefilled_response>

Please provide the survey data and any historical comparison data for analysis.

[Survey data attached]
[Historical data attached]
[Competitive benchmark data attached]
```

## Advanced iteration techniques (RSTI)

### Revisit TCREI fundamentals

Even at master level, we circle back to ensure:

**Task:** Multi-phase collaborative analysis with specific deliverables

**Context:** Board-level decision with $10M+ implications

**Reference:** Historical data, competitive benchmarks, industry standards

**Evaluate:** Success metrics clearly defined with ROI thresholds

## Techniques added at the master level

- **Iterate:** Use scenario planning to explore several plausible interpretations.
- **Separate:** Break a complex analysis into focused prompts and pass each result to the next step.
- **Reframe:** Test the task through analogies that encourage different perspectives.
- **Constrain:** Include real limits on budget, time, engineering capacity, regulation, and acceptable risk.

## Example prompt chain

1. **Data foundation — Dr. Chen:** Analyze data quality, calculate statistical significance for the major
   findings, and identify the five most reliable insights with confidence intervals.
2. **Behavioral analysis — Marcus:** Given the statistical findings, identify the behavioral patterns and
   likely customer motivations behind them.
3. **Strategic translation — Elena:** Translate the behavioral insights into business implications, resource
   requirements, and return-on-investment projections for the three strongest recommendations.
4. **Synthesis — full team:** Resolve conflicting interpretations and produce one set of recommendations with
   an implementation timeline.

## Alternative task framings

- **Detective:** Treat the satisfaction change as a mystery. Identify clues, form a theory, and test the
  proposed cause against the evidence.
- **Medical diagnosis:** Treat the product as a patient. Use the survey as symptoms, diagnose likely causes,
  and prescribe interventions with expected recovery times.
- **Investment analysis:** Treat the product work as a $10 million investment. Use the survey for due
  diligence and recommend where capital should go for the best risk-adjusted return.

## Constraints to make explicit

### Resources

- Two senior developers.
- A quarterly budget cap of $500,000.
- An eight-week deadline before the holiday season.
- Existing technical debt.

### Business environment

- Regulatory compliance.
- Dependencies on existing user workflows.
- Likely competitive responses.
- Platform limits such as Apple's review cycle.

### Success criteria

- Minimum improvement thresholds.
- Return-on-investment requirements.
- Acceptable risk levels.
- Stakeholder approval requirements.

## Evolution from level 3 to level 4

The master-level prompt adds multiple expert perspectives, competing scenarios, systematic verification,
real-world constraints, implementation planning, and explicit risk handling. It turns a request for analysis
into a structured decision-support process.
