# Voice guide: how I talk upstream

## Who I am in threads

I'm a CodePath AI301 student making my first contribution to Path Review, focused on accessibility and frontend testing. I work with AI assistance and verify everything I post. Expect short comments with evidence, and questions when I'm unsure.

## Rules I write by

### Rule: promise the look, not the fix

Say what I will investigate next. Never promise a fix, a pull request, or a date.

- Wrong: "I'll have jest-axe tests up in a PR by the weekend!"
- Right: "Next I'll run the ReviewPage tests with jest-axe added and post what it reports."

### Rule: say what I ran

Only write "reproduced" or "confirmed" next to pasted output. Otherwise say what I tried and what happened.

- Wrong: "Confirmed, the review page has accessibility problems."
- Right: "I ran `npm test -- ReviewPage` and there is no axe check in that file; output below."

### Rule: name the specifics

Every comment names this issue's actual behavior and files, not general enthusiasm.

- Wrong: "Happy to help with this one!"
- Right: "I'd like to add the jest-axe check to `frontend/src/pages/__tests__/ReviewPage.test.tsx` that this issue asks for."

### Rule: disclose AI once, plainly

State my AI use once per thread, in my first comment, even when the repo doesn't require it. Don't repeat it in later comments unless the kind of help changes. A repo policy that requires disclosure on every comment overrides this.

- Wrong: repeating "(AI-assisted, as mentioned above)" in every comment on the thread
- Right: the claim comment says "I work with AI assistance (Claude) and verify every command and output myself", and the repro comment doesn't mention it again

## Things I never post

- Dates or deadlines ("by Friday", "this weekend", "shortly").
- Text an AI drafted that I haven't read line by line and can explain.
- Guesses about difficulty before I've looked at the code ("should be easy").
- Piggyback repros ("same as above, can confirm") in place of my own evidence.
