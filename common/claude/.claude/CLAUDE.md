# Global Rules

## Research-Before-Edit Protocol
Before modifying ANY file, you MUST:
1. Read the target file completely (or the relevant section for very large files)
2. Read files that import from or are imported by the target
3. Grep for all usages of any function, class, or variable you plan to change
4. Read relevant test files if they exist
5. Only THEN propose or make changes

NEVER edit a file you have not read in this session.
NEVER rewrite an entire file when a targeted edit will do.

## Completion Standards
- Complete the full task. Do not stop early or ask "should I continue?" unless genuinely ambiguous.
- Do not claim code is fixed without verifying (run tests, check compilation, etc.).
- If you encounter an error, debug it fully before reporting back.
- Show reasoning before making changes on complex tasks.

## Quality Gates
- Every edit must be preceded by reading the file.
- Prefer targeted line edits over full-file rewrites.
- If modifying a function, grep for all callers first.
- Run existing tests after making changes when test infrastructure exists.
- When a task involves multiple files, map the dependency chain before starting edits.
