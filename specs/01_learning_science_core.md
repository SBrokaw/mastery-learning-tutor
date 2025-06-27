# Learning Science Core Specification

## Overview

This specification defines the core learning science principles that form the foundation of the Mastery Learning Career Coach system, based on the proven methodologies from Math Academy.

## Bloom's Two-Sigma Problem

### Problem Statement
Traditional classroom instruction leaves significant learning potential unrealized. One-on-one tutoring produces vastly better outcomes (average tutored student performs better than 98% of traditional classroom students), but is prohibitively expensive at ~$13,000/year.

### Solution Approach
Combine evidence-based learning strategies with technology to achieve tutoring-level effectiveness at scale, making talent development accessible to serious learners.

## Core Learning Principles

### 1. Active Learning
**Definition**: Students actively engage in learning exercises rather than passively consuming content.

**Implementation Requirements**:
- Students solve problems within minutes of starting lessons
- Majority of time (80%+) spent on active problem-solving
- Immediate feedback and correction
- Every student actively engaged on every piece of material

**Evidence Base**:
- Meta-analysis shows active learning superior to traditional lecturing across STEM subjects
- Students in active classes learn more despite perceiving they learned less
- Cognitive effort during active learning is a sign of effective learning, not poor instruction

### 2. Mastery Learning
**Definition**: Students must demonstrate proficiency before advancing to more complex topics.

**Implementation Requirements**:
- Diagnostic assessment of current knowledge
- Prerequisite-based topic sequencing
- Performance-based advancement gates
- Continuous knowledge validation through spaced repetition

**Evidence Base**:
- Students grouped by ability rather than age
- Each student learns each skill to sufficient threshold before advancing
- Progress measured by advanced skills executable to performance threshold

### 3. Spaced Repetition
**Definition**: Systematic review of previously learned material at optimal intervals to prevent forgetting.

**Implementation Requirements**:
- Fractional Implicit Repetition (FIRe) algorithm
- Hierarchical knowledge credit flow
- Individualized timing based on student-topic learning speed
- Automatic review scheduling

**Evidence Base**:
- Spacing effect: more long-term retention with spaced vs. massed practice
- Repetition compression: reviews become less frequent over time
- Consensus among researchers on effectiveness

### 4. Deliberate Practice
**Definition**: Focused, effortful practice on specific skills beyond current comfort zone.

**Implementation Requirements**:
- Individualized training activities for specific performance aspects
- Effortful repetition with successive refinement
- Focus on areas beyond current repertoire
- Structured feedback and correction

**Evidence Base**:
- Expertise is product of incremental improvements over time
- Motivational supplements are not substitutes for deliberate practice
- Requires effort and discomfort for optimal learning

### 5. Knowledge Graph
**Definition**: Hierarchical representation of topic relationships with prerequisite mapping.

**Implementation Requirements**:
- Direct prerequisites: immediate requirements
- Key prerequisites: critical foundational skills
- Encompassing relationships: advanced topics practice simpler skills
- Partial encompassings: fractional credit for component skills

**Evidence Base**:
- Enables targeted remediation for missing foundations
- Supports turbo-boosted learning through encompassing relationships
- Facilitates efficient diagnostic assessment

## Individual Differences

### Learning Speed vs Learning Style
- People differ in learning speed, not learning style
- Working memory capacity impacts perceived effort and abstraction ability
- Different students need different amounts of practice
- All can learn some, many can learn more, few can learn all

### Mathematical Potential
- Learning energy vs level of math relationship
- Abstraction ceiling exists but is likely higher than commonly believed
- Struggle does not imply inability (can be caused by missing foundations, ineffective practice, insufficient practice, or lack of motivation)

## Desirable Difficulty

### Concept
Learning should be effortful but not overwhelming. Optimal difficulty creates "desirable difficulty" that promotes learning.

### Implementation
- Active learning creates cognitive effort that students may misinterpret as poor learning
- Immediate feedback prevents frustration from insurmountable obstacles
- Progressive difficulty scaling based on performance
- Balance between challenge and success

## Technology Requirements

### Adaptive Learning Engine
- Real-time assessment of student knowledge
- Dynamic content selection based on performance
- Individualized pacing and difficulty adjustment
- Continuous optimization of learning path

### Knowledge Tracking
- Granular topic-level mastery tracking
- Performance history and trend analysis
- Prerequisite relationship enforcement
- Spaced repetition scheduling

### Content Management
- Modular lesson structure
- Problem generation and validation
- Feedback and explanation systems
- Progress visualization

## Success Metrics

### Learning Efficiency
- Time to mastery of target skills
- Retention rates over time
- Transfer of learning to new contexts
- Confidence in skill application

### Engagement Metrics
- Active learning time percentage
- Problem completion rates
- Review participation rates
- Session duration and frequency

### Mastery Validation
- Diagnostic assessment accuracy
- Prerequisite mastery verification
- Advanced skill demonstration
- Long-term retention testing

## Implementation Guidelines

### Content Development
- Start with minimum effective dose of explanation
- Follow immediately with active practice
- Provide immediate feedback and correction
- Scale difficulty based on performance

### Assessment Design
- Focus on skill demonstration, not content recall
- Include prerequisite validation
- Measure both accuracy and speed
- Provide detailed feedback for improvement

### User Experience
- Clear progress indicators
- Transparent mastery requirements
- Immediate feedback on performance
- Intuitive navigation through knowledge graph

## References

- Bloom, B. S. (1984). The 2 sigma problem: The search for methods of group instruction as effective as one-to-one tutoring.
- Freeman, S., et al. (2014). Active learning increases student performance in science, engineering, and mathematics.
- Brown, P. C., Roediger, H. L., & McDaniel, M. A. (2014). Make it stick: The science of successful learning.
- Math Academy Way - Cognitive Learning Strategies 