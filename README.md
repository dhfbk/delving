This repository contains the data release for the paper *Delving into Qualitative Implications of Synthetic Data for Hate Speech Detection* at EMNLP 2024.

In this repository, we release the set of data that was manually annotated (3,500 examples in total). This subset does not include the original texts from the Measuring Hate Speech Corpus (Kennedy et al., 2020; Sachdeva et al., 2022), but only their comment IDs from the original dataset and the aggregated hate speech label we calculated for them, so the source MHS corpus should be first retrieved if one wishes to pair the source texts with their synthetic version.
[Here](https://huggingface.co/datasets/ucberkeley-dlab/measuring-hate-speech) is a link to the Measuring Hate Speech Corpus. 

The annotation was divided into two sets:
- 500 examples annotated as human or llm-written. These are contained in the file `human-vs-llm.tsv`, which contains 5 columns:
	- `comment_id`: the original comment id that can be used to retrieve the original text from the MHS corpus.
	- `label_x`: the *hate speech* label we calculated after the aggregation process for the text corresponding to `comment_id`, based on the annotations of the MHS corpus (`0` means no hate speech, `1`means hate speech).
	- `author`: the source of the `text`. It is a string with the name of the LLM if the text was LLM-written (`llama`,`mistral`, or `mixtral`) or `gold` if it is an original real-world text.
	- `text`: the paraphrased or original text annotated by our annotators.
	- `LLM?`: the annotation by our annotators. It is `TRUE` if the annotator marked the text as LLM-written, `FALSE` if they thought it was a human.
- 3,000 examples (1,000 per model) annotated according to the other aspects we analyzed in the paper. These annotations are found in the files `annotations-llama2-chat-7b.tsv`, `annotations-mistral-7b.tsv`, and `annotations-mixtral-8x7b.tsv`. These files each contain 1,000 lines with 14 columns:
	- `comment_id`: the original comment id that can be used to retrieve the original text from the MHS corpus.
	- `label_x`: the *hate speech* label we calculated after the aggregation process for the text corresponding to `comment_id`, based on the annotations of the MHS corpus (`0` means no hate speech, `1`means hate speech).
	- `synth_text`: a string containing the LLM-generated paraphrase of the original text.
	- `prompt_failure`: whether the annotators deemed that the model was not able to correctly fulfill the instructions, and if so, the type of failure. Can be `FALSE` , `Prompt failure`, or `Description of original gold.
	- `hate_speech`: whether our annotators found the synthetic text to contain hate speech or not. Can be `No` (no hate), `Yes`(hateful), or `Unclear`.
	- `grammar_ok`: if the synthetic text was deemed grammatically correct/realistic or not. Can be `Yes` or `No`.
	- `world_knowledge_correct`: whether the world knowledge present in the synthetic text is ok/realistic. Can be `Yes` or `No`.
	- target information: for each target category *t* in [*origin*, *race*, *religion*, *gender*, *sexuality*, *age*, and *disability*], there is a column `target_`\[*t*\], which is `FALSE` if that target is not present in the synthetic text, or a string detailing which target type it is if there is one under that category. We use the same targets as the original MHS corpus where possible for all categories but *origin*, since it can get extremely sparse, for which we only use the `TRUE`value if relevant.