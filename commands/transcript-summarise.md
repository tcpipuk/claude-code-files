I need you you to process the provided meeting transcript to produce two key documents:

1. **transcript_summary.md** - Comprehensive meeting summary with full context and analysis
2. **transcript_actions.md** - Concise action tracker for follow-up and accountability

If it's unclear which transcript I want summarised, hard stop here and ask me which file to use.

## Language and Style Requirements

- **UK English orthography** throughout all documents
- Use "e.g." not "e.g.," (avoid overly pedantic punctuation)
- Professional business tone, approachable but technically accurate
- Clear, direct language avoiding unnecessary jargon
- Markdown formatting following markdownlint standards

## Document 1: Meeting Summary (meeting_summary.md)

### Summary Structure

1. **Header**
   - Meeting title/subject
   - Date and time
   - Purpose statement

2. **Participants**
   - Group by organisation/team
   - Include roles/titles where identifiable
   - Note participation level if relevant

3. **Executive Summary**
   - 2-3 sentence overview of meeting outcome
   - Key decision or agreement reached

4. **Main Content Sections**
   - Product/Service Demonstrated (if applicable)
   - Main Discussion Points and Concerns
   - Technical Details Covered
   - Commercial/Strategic Topics

5. **Participant Engagement** (optional but valuable)
   - Note who was highly engaged vs passive
   - Identify decision makers vs contributors
   - Observe any tensions or disagreements

6. **Agreed Actions and Next Steps**
   - Organised by responsible party
   - Include timelines where mentioned
   - Note dependencies between actions

7. **Outstanding Items**
   - Questions raised but not answered
   - Topics deferred to offline discussion
   - Items requiring further clarification

8. **Conclusion**
   - Meeting success assessment
   - Critical success factors
   - Risks identified

### Methodology for Summary

- **Extract facts carefully** - Transcripts often contain errors; use context to determine truth
- **Identify key themes** - Group related discussion points together
- **Capture nuance** - Note scepticism, enthusiasm, concerns even if not explicitly stated
- **Include specific details** - Percentages, thresholds, technical specifications mentioned
- **Preserve important quotes** - Especially for concerns or commitments made

### Example Summary Section

```markdown
### Technical Capabilities Discussed

**Performance Monitoring**
The team demonstrated real-time monitoring capabilities with sub-second polling intervals.
Key metrics include CPU utilisation, memory usage, and network throughput with historical
trending over 90-day periods.

**Integration Requirements**
The solution requires API access to existing systems. Concerns were raised about
authentication methods, with the security team requesting OAuth2 implementation rather
than API keys. This will be investigated further offline.
```

## Document 2: Action Tracker (meeting_actions.md)

### Tracker Structure

1. **Header**
   - Meeting date
   - Purpose: "Quick reference for follow-up and accountability"

2. **Actions by Organisation/Team**
   - Group actions by company/department
   - Sub-group by person where multiple people have actions
   - Use bullet points for each action
   - Include key details (quantities, deadlines) inline

3. **Pending Clarification**
   - Items needing further discussion
   - Questions without owners
   - Technical specs to be determined

4. **Next Milestone**
   - Single line with next critical date/deliverable

### Methodology for Actions

- **Focus on commitments** - Only include what people agreed to do
- **Assign clear ownership** - Name specific individuals where possible
- **Be specific** - Include numbers, percentages, technical details mentioned
- **Keep concise** - One line per action, no explanatory text
- **Identify dependencies** - Note when one action blocks another

### Example Action Section

```markdown
## SUPPLIER ACTIONS

**Technical Team Lead**
- Provide architecture diagram by end of week
- Schedule technical deep-dive session with customer architects
- Investigate OAuth2 integration feasibility (security team requirement)

**Account Manager**
- Draft commercial proposal including three pricing tiers
- Arrange executive briefing session for board approval

## CUSTOMER ACTIONS

**IT Director**
- Approve proof-of-concept environment specifications
- Nominate technical team members for integration workshop
- Provide access to test environment by specified date
```

## Processing Methodology

### Step 1: Initial Read

- Identify meeting type (sales demo, technical review, project update, etc.)
- Note participant count and roles
- Understand primary objective

### Step 2: Information Extraction

- Create timeline of discussion topics
- Identify all commitments made
- Note specific requirements or specifications mentioned
- Capture concerns, objections, or risks raised

### Step 3: Verify Through Context

- Cross-reference participant statements for consistency
- Identify and resolve transcription errors using context
- Distinguish between suggestions and firm commitments

### Step 4: Organisation

- Group related topics together
- Separate strategic from tactical discussions
- Identify clear action owners

### Step 5: Quality Checks

- Ensure all actions have owners
- Verify timeline references are specific
- Confirm technical details are accurately captured
- Check for contradictions or unclear commitments

## Common Transcription Issues to Navigate

### Speech Recognition Errors

- Company names often transcribed incorrectly (use context to determine correct spelling)
- Technical terms may be garbled (use industry knowledge to interpret)
- Numbers might be wrong (check if "fifteen" vs "fifty" makes sense in context)

### Meeting Dynamics

- People talking over each other (identify separate threads)
- Side conversations in chat (note as "via chat" if relevant)
- Participants joining/leaving (note if it affects decisions)

### Example Interpretation

```plain
Transcript: "So we clown out. We we we've recently switched to..."
Interpretation: Speaker likely said "So we found out" or "So to round out"
Use context of following sentences to determine most likely meaning.
```

## Output File Guidelines

### meeting_summary.md

- Length: Comprehensive but focused (typically 150-250 lines)
- Sections: Use clear hierarchy with ##, ###, ####
- Lists: Use bullets for items, numbered for sequential steps
- Emphasis: Bold for names/roles, italics sparingly

### meeting_actions.md

- Length: Concise (typically 40-80 lines)
- Format: Minimal formatting, maximum clarity
- Focus: Who does what by when
- No fluff: No explanatory paragraphs, just actions

## Final Notes

- Maintain objectivity - Report what was said, not what should have been said
- Include emotional temperature where relevant (enthusiasm, scepticism, frustration)
- Flag any commitments that seem unrealistic or contradictory
- If critical information is unclear, note it explicitly rather than guess
- Always preserve specific numbers, percentages, and technical specifications exactly as mentioned
