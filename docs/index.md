# Preserve exact-answer notation when building an exam review tool

An exam review tool should retain what the learner entered before normalising it. A decimal, a fraction and a symbolic expression may be equivalent in one question and unsuitable in another. The response format belongs to the question contract. A global clean-up function that silently changes notation can hide the reason an answer was accepted or rejected.

For a small prototype, keep three separate fields: raw input, a documented normalised form, and the question's expected format. Trimming surrounding whitespace is usually easy to explain. Rounding a decimal, simplifying an expression or converting an interval needs a rule that can be inspected. Store the rule version with the result so later changes do not rewrite the evidence of an earlier check.

The [2026 Shanghai mathematics paper preview](https://gaokaoexam.org/subjects/math/full-exams/2026-shanghai) provides a concrete mix of response tasks for studying these boundaries. The public experience contains 21 questions, uses a 120-minute limit and has a 150-point total. It is independently digitized and translated. Treat it as an example to inspect, not an official specification for every Gaokao paper.

Here is a deliberately narrow helper for a teaching prototype. It preserves the original string and removes only surrounding whitespace. It does not grade answers, determine algebraic equivalence, or modify the live GaokaoExam.org product.

```python
def capture_answer(text):
    if not isinstance(text, str):
        raise TypeError("answer text must be a string")
    return {
        "raw": text,
        "trimmed": text.strip(),
        "normalization_rule": "outer-whitespace-v1",
    }

sample = capture_answer("  1/2  ")
assert sample["raw"] == "  1/2  "
assert sample["trimmed"] == "1/2"
```

Add independent examples before expanding the rule. Include a sign, an exact fraction, a symbolic variable and a malformed response. Decide explicitly which changes are formatting and which require mathematical reasoning. Keep written-proof review outside this string-handling helper. A preserved answer is evidence for review; it is not a mark.
