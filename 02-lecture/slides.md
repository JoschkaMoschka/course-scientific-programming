# Software development

---


[Software development](https://en.wikipedia.org/wiki/Software_development) is the process of designing, creating, testing, and maintaining software applications to meet specific user needs or business objectives.

---


## Software development lifecycle


- Defining goals
- Evaluating feasibility
- Analyzing requirements
- Architecture and design
- Implementation (coding)
- Testing
- Deployment
- Maintenance

===

## Requirements engineering

Before writing any code, it's essential to clearly define:

- what your program is supposed to do,
- what data it needs,
- what output it should generate, and
- how you'll know it's working correctly.

This process is called **requirements engineering**.

---

## What is the goal?

- What is the research question?
- Which problem is to be solved?
- Which questions are to be answered?
- What are the assumptions?
- What are constraints and limitations?

---

## What is the required input?

- What type of data is required?
- Where does the data come from?
- What are the expectations for quality, correctness, and completeness?
- What is the expected format and structure?
- What kind of pre-processing of input data is required? 

---

## What is the required output?

- What are the expected results?
- Which data is generated?
- What format and structure is appropriate?

---

## How do you know it works?

- How will the program be tested?
- What are the correctness criteria?
- How can the results be validated or verified?

===

## Architecture and design

- Software **architecture** defines the high-level structure of a software system, i.e. how components are organized and interact.
- Software **design** is the detailed plan for how those components are implemented.

---

### UML component diagrams

A [UML component diagram](https://en.wikipedia.org/wiki/Component_diagram) shows the components of a larger system and the interfaces used to connect these components.

<!--
@startuml
  [Data provider] as DP
  [Controller] as C
  [Observer] as O
  [Model] as M
  interface "input" as IInput
  interface "control" as IControl
  interface "subscribe" as ISubscribe
  interface "update" as IUpdate
  DP .right.> IInput
  IInput -right- M
  M -down- ISubscribe
  O .up.> ISubscribe
  O -up- IUpdate
  M .down.> IUpdate
  C .left.> IControl
  M -right- IControl
}
@enduml
-->

![UML](02-lecture/Component_diagram.svg)<!-- .element style="height:400px;" -->


> [!TIP]
> Component diagrams can be created with [https://plantuml.com/component-diagram](https://plantuml.com/component-diagram)

---

### Architectural principles

- **Separation of concerns:** Each component (e.g., data input, model logic, control logic, or output) is responsible for a single well-defined task.
- **Modularity:** It should be possible to replace or modify components without affecting the whole

===

### State machines

A [state machine](https://en.wikipedia.org/wiki/Finite-state_machine) represents systems by 

- **states** representing the characteristics of the system at a point in time,
- **state transitions** defining how the system can move from one state to another, and 
- **conditions** or **triggers** that cause these transitions.

> [!NOTE]
> State machines are quite common in scientific computing, in particular, when simulating complex systems.

---

### UML state machine diagrams

A [UML state machine diagram](https://en.wikipedia.org/wiki/UML_state_machine) depicts states, transitions, and conditions/triggers.

<!--
@startuml
[*] -> Initialized
Initialized -> Running : start
Running -d-> Paused : pause
Running -> Completed : finish
Completed -d-> [*]
Paused -> Aborted : abort
Running -> Aborted : abort
Paused -u-> Running : resume
Aborted -r-> [*]
@enduml
-->

![UML](02-lecture/State_machine_diagram.svg)<!-- .element style="height:400px;" -->


> [!TIP]
> State machine diagrams can be created with [https://plantuml.com/state-diagram](https://plantuml.com/state-diagram)

---

### UML class diagrams

[UML class diagrams](https://en.wikipedia.org/wiki/Class_diagram) represent the data and objects model of a program.

<div class="twocolumn" style="align-items:center;">
<div>
<!--
@startuml
object Simulator {
  +parameters: Parameters
  +current_state: State
  +logger: Logger
}
object Parameters {
  +duration: Int32
  +arrival_rate: Float64
  +service_rate: Float64
  +number_of_servers: Int32
}
object State {
  +time: Float64
  +queue_length: Int32
  +busy_servers: Int32
}
object Logger {
  +log: Vector{State}
}
Simulator -- Parameters
Simulator -- State
Simulator -- Logger
Logger - State
@enduml
-->

![UML](02-lecture/Class_diagram.svg)

</div>
<div>
They include:

- Classes/structs
- Attributes (fields)
- Methods (functions operating on the class)
- Inheritance and composition

</div>
</div>

> [!TIP]
> Class diagrams can be created with [https://plantuml.com/class-diagram](https://plantuml.com/class-diagram)

---

### UML activity diagrams

<div class="twocolumn" style="align-items:center;">
<div>
<!--
@startuml
start
:Initialize system state;
repeat
  :Wait for event;
  if () then (arrival)
    :Add entity to queue;
  else (departure)
    :Remove entity from queue;
  endif
  :Update system state;
repeat while () is (continue) not (terminate)
stop
@enduml
-->

![UML](02-lecture/Activity_diagram.svg)<!-- .element style="height:500px;" -->


</div>
<div>

[UML activity diagrams](https://en.wikipedia.org/wiki/Activity_diagram) describe workflows, i.e. sequences of operations and decisions.

</div>
</div>

> [!TIP]
> Class diagrams can be created with [https://plantuml.com/activity-diagram-beta](https://plantuml.com/activity-diagram-beta)

===

## Implementation

A typical software development workflow looks like this: 

1. **Issues:** Define a task, feature, or bug.
2. **Forking:** Create your own copy of a repository to contribute to projects you do not own.
3. **Branching:** Develop new feature or fixes in isolated branches without affecting the main branch.
4. **Commits:** Save meaningful progress with context.
5. **Pull Requests:** Propose changes to be merged into the main branch.
6. **Code Review:** Peers review your code for correctness, readability, performance, and style.
7. **Merge and deploy:** Integrate tested code into the main branch and release new version.


===

## Continuous integration and continuous deployment (CI/CD)

[CI/CD](https://en.wikipedia.org/wiki/CI/CD) automates the building, testing, and deployment of software:

- **Continuous Integration (CI):** Automatically build and test your code every time you push changes.
- **Continuous Deployment (CD):** Automatically deploy new versions after passing tests.

> [!TIP]
> CI/CD is especially helpful in scientific programming to ensure that results are reproducible, models stay valid, and documentation is up to date.

---

## GitHub Actions

GitHub can be configured to automatically runs workflows defined through [YAML](https://en.wikipedia.org/wiki/YAML) files in a folder named `.github/workflows/`.

> [!TIP]
> The group projects are cloned from https://github.com/KLU-BADS/ProjectTemplate.jl and include workflows that are automatically run.

---

### CI for automatic testing

```yaml
name: Tests

on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

# Cancel an in-progress run when a new commit is pushed to the same branch.
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  test:
    name: Julia ${{ matrix.julia-version }} - ${{ matrix.os }}
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        julia-version: ['1']
        os: [ubuntu-latest]

    steps:
      - uses: actions/checkout@v4

      - uses: julia-actions/setup-julia@v2
        with:
          version: ${{ matrix.julia-version }}

      - uses: julia-actions/cache@v2

      - uses: julia-actions/julia-buildpkg@v1

      - uses: julia-actions/julia-runtest@v1
```

> [!NOTE]
> Group projects are pre-configured to automatically run tests. You are expected to write tests that ensure that your project is doing what it is supposed to do. **Never let tests fails in `main`!** Testing will be part of the final assessment.

---

### CD for automatic deployment of documentation

```yaml
name: Documentation

on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

# Allow one deployment at a time; do not cancel a run that is already
# publishing.
concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    name: Build documentation
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: julia-actions/setup-julia@v2
        with:
          version: '1'

      - uses: julia-actions/cache@v2

      # PlantUML renders the diagrams by running plantuml.jar.
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'

      - name: Install documentation dependencies
        run: |
          julia --project=docs -e '
            using Pkg
            Pkg.instantiate()'

      - name: Build documentation
        run: julia --project=docs docs/make.jl

      - name: Upload Pages artifact
        # Only on main: pull request runs build the docs to catch errors,
        # but do not publish them.
        if: github.ref == 'refs/heads/main' && github.event_name != 'pull_request'
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/build

  deploy:
    name: Deploy to GitHub Pages
    needs: build
    if: github.ref == 'refs/heads/main' && github.event_name != 'pull_request'
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy
        id: deployment
        uses: actions/deploy-pages@v4
```

> [!NOTE]
> Group projects are pre-configured to build and deploy documentation. This documentation will be part of the final assessment.


===

## Refactoring

Refactoring means restructuring existing code without changing its external behavior. 

> [!NOTE]
> The goal is to improve:
> - **Readability** (e.g., clearer names, better organization)
> - **Maintainability** (e.g., modular design, avoiding duplication)
> - **Performance** (e.g., replacing inefficient patterns)

> [!TIP]
> For every newly implemented functionality try to simplify and improve code quality before continuing. Always run tests after refactoring to ensure nothing breaks.
