# /map-states

Model all states of a complex UI component.

## Steps
1. **Enumerate** — List every possible state the component can be in using `state-machine` skill.
2. **Transitions** — Define what events move between states using `state-machine` skill.
3. **Loading** — Model async and loading states using `loading-states` skill.
4. **Errors** — Model error and recovery states using `error-handling-ux` skill.
5. **Feedback** — Define feedback for each transition using `feedback-patterns` skill.
6. **Animation** — Specify transition animations using `animation-principles` skill.

## Output
State machine diagram with states, events, transitions, guards, actions, and corresponding UI for each state.

Consider following up with `/design-interaction` for detailed interaction specs.
