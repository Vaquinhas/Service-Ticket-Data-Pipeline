# Mini-Project 1 — Service Ticket Data Pipeline Report

## Diagnosis and Observation Unit
The source dataset `service_tickets.csv` contains `[raw_count]` rows, where each row represents a single support ticket observation expected to be uniquely identified by `ticket_id` (`T` followed by three digits). Initial inspection separated values already missing in the source from present-but-invalid entries, and distinguished harmless exact row copies (`[exact_duplicates_removed]` removed) from repeated identifiers with conflicting fields.

## Cleaning and Transformation Rules
After removing exact duplicate rows without mutating the source file, rows were rejected if they had missing, malformed, or conflicting IDs, or missing, unparseable, or out-of-October-2026 dates (`[rejected_count]` rows rejected, each counted once with all applicable reasons preserved in `outputs/rejected_tickets.csv`). For the `[retained_count]` retained rows:
- `channel` and `priority` were stripped, lowercased, and mapped to `"unknown"` when missing or unrecognized (`[channel_unknown_count]` unknown channels; `[priority_unknown_count]` unknown priorities), retaining Boolean flags.
- `handling_minutes` accepted finite nonnegative values (including zero and decimals); `[handling_imputed_count]` missing or invalid durations were flagged (`handling_imputed`) and imputed with the median of valid retained durations (`[handling_fill_median]` minutes).
- `satisfaction` retained only integer ratings from 1 to 5; `[satisfaction_unavailable_count]` missing or invalid ratings were set to missing and flagged (`satisfaction_unavailable`) without imputation to avoid inventing customer opinions.
- `handling_zscore` was computed using population standard deviation (`ddof=0`, mean `[handling_mean]`, std `[handling_std_pop]`) with a safe denominator of 1 for constant columns, and exported with the four one-hot channel indicators in `outputs/ticket_features.csv` (excluding `satisfaction`).

## Verification and Limitations
Row balance (`[raw_count] = [exact_duplicates_removed] + [rejected_count] + [retained_count]`), schema checks, edge-case tests (constant duration, conflicting IDs, multi-reason rejections, and no-valid-duration error), and disk read-back checks all passed.

Two key limitations remain:
1. **Imputation bias:** Median imputation for `handling_minutes` preserves sample size but compresses variance and assumes missing durations do not systematically differ from observed ones.
2. **Descriptive scaling only:** The imputation median, mean, and standard deviation were computed on the full retained table; for future predictive modelling, these statistics must be fitted exclusively on training splits to avoid data leakage.

## How to Run and Assistance
Install Python 3 with `numpy` and `pandas`, keep `service_tickets.csv` in the repository root, and run all cells of `project_1_student.ipynb` from top to bottom to regenerate all files in `outputs/`. AI assistance (Gemini) was used to review alignment with the brief, refine edge-case tests, and structure this report; all code and outputs were executed and verified locally.
