# Spaced Repetition Scheduler Specification

## Overview

This specification defines how the system schedules review tasks using spaced repetition principles to optimize long-term retention. The system creates simple review tasks in markdown format rather than implementing complex algorithms.

## Core Concept

Spaced repetition systematically schedules reviews of previously learned material at optimal intervals to prevent forgetting. Instead of building complex software, we create simple review tasks that follow proven timing patterns.

## Review Scheduling Principles

### Basic Intervals
- **Initial Review**: 1-2 days after learning
- **Second Review**: 1 week after initial review  
- **Third Review**: 1 month after second review
- **Subsequent Reviews**: Increasing intervals (2 months, 4 months, etc.)

### Review Task Creation
When a learning task is completed, the system automatically creates future review tasks:

```markdown
# Review Task: Linear Algebra - Matrix Operations
**Type**: Review
**Resource**: Previous notes and practice problems
**Objective**: Maintain mastery of matrix operations
**Due Date**: [Calculated based on spaced repetition schedule]
**Success Criteria**: Complete 5 practice problems with 90% accuracy
**Notes**: This is your 2nd review of this topic (1 week after initial learning)
```

### Review Task Types

#### Practice Reviews
- Complete a small set of practice problems
- Focus on core concepts and common applications
- Verify retention without reteaching

#### Self-Assessment Reviews
- Quiz yourself on key concepts
- Identify any forgotten material
- Plan additional practice if needed

#### Application Reviews
- Apply the skill to a new context
- Connect to related topics
- Build deeper understanding

## Implementation Approach

### Simple Scheduling
- Store review due dates in markdown task files
- Calculate intervals based on completion history
- Adjust timing based on performance feedback

### Review Task Management
- Create review tasks automatically when learning tasks complete
- Track review completion and performance
- Adjust future intervals based on success/failure

### Integration with Task System
- Review tasks appear in regular task lists
- Prioritize reviews that are overdue
- Link reviews to original learning tasks

## Success Metrics

### Retention Effectiveness
- Performance on review tasks over time
- Long-term retention rates
- Forgetting curve validation

### User Experience
- Review task completion rates
- User satisfaction with review timing
- Perceived value of review tasks

## References

- Math Academy spaced repetition methodology
- Spaced repetition research literature
- Simple task management best practices 