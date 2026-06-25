
------------------------------
## 📋 Pair Programming Interview Framework## 1. The Setup (First 2 Mins)

* Clarify Constraints: Ask, "Can I assume the input is already sanitized and passed as a clean Python list, or do you want me to write a string/input parser first?" (Bypasses input processing trap).
* Define Data Structures: State them before typing.
* Items: List[int]
   * Containers: List[int] (Tracks remaining space per container index).

## 2. Coding Strategy: "Chunk & Silence"
Do not speak and write complex logic simultaneously. Use three steps for every block:

   1. State Intent: "I am going to write the loop that searches for an available container."
   2. Go Silent: Take 30–45 seconds of absolute silence to type the code block perfectly.
   3. Confirm & Explain: "Done. This block iterates through existing containers and checks if the item fits. Now I will handle the fallback case."

## 3. Core Logic Blueprints (Pre-Memorised)
Never invent loop structures on the fly. Rely on these automated templates:
## Linear Search/Allocation (e.g., FFD)
```
External loop: Itemsfor item in sorted_items:
    placed = False
    # Internal loop: Existing Resources
    for i in range(len(resource_caps)):
        if resource_caps[i] >= item:
            resource_caps[i] -= item
            placed = True
            break
    # Fallback: Allocate New Resource
    if not placed:
        resource_caps.append(max_capacity - item)
```
## Fast Input Parsing (If Forced)

Comma-separated string to clean int 
```
listitems = [int(x) for x in input_str.split(',') if x.strip()]
```

## 4. Key Milestones to Hit

* 0–2 Mins: Validate inputs/bounds + negotiate away heavy parsing.
* 2–5 Mins: State data structures out loud; write a 3-line comment skeleton.
* 5–15 Mins: Code the core logic using "Chunk & Silence".
* 15–20 Mins: Dry-run your code with the interviewer's sample input.
