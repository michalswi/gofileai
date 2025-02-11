# gofileai

![](https://img.shields.io/github/stars/michalswi/gofileai)
![](https://img.shields.io/github/issues/michalswi/gofileai)
![](https://img.shields.io/github/forks/michalswi/gofileai)
![](https://img.shields.io/github/last-commit/michalswi/gofileai)
![](https://img.shields.io/github/release/michalswi/gofileai)

```
$ ./gofileai -h
Usage: ./gofileai [options]
  -f string
    	Path to the file to be reviewed [required]
  -file string
    	Path to the file to be reviewed [required]
  -m string
    	Message to OpenAI model [required OR use '-p']
  -message string
    	Message to OpenAI model [required OR use '-p']
  -o	Save file's review output to a file [optional]
  -out
    	Save file's review output to a file [optional]
  -p string
    	Pattern name
  -pattern string
    	Pattern name
  -rag
    	Enable Retrieval-Augmented Generation (RAG) [optional]
  -v	Display OpenAI model version
  -version
    	Display OpenAI model version
```

OpenAI default model version used **o1-mini** .  
To use different model please refer to documentation [here](https://pkg.go.dev/github.com/sashabaranov/go-openai#pkg-constants) .
```
$ ./gofileai -v
o1-mini

$ OPENAI_MODEL="o1-preview" ./gofileai -v
o1-preview
```

You need [OpenAI API key](https://platform.openai.com/api-keys) to interact with OpenAI API.

```
$ export API_KEY=<>
$ ./gofileai (...)
```


### **IMPORTANT**  

For example for OpenAI in version **GPT-4.0** .

If you encounter such error, it's because there are some API limitations.
```
Request too large for gpt-4 in organization <org> on tokens per min (TPM): Limit 10000, Requested 43034. The input or output tokens must be reduced in order to run successfully. Visit https://platform.openai.com/account/rate-limits to learn more.
```
More about **Rate limits** for **tier-1** you can find [here](https://platform.openai.com/docs/guides/rate-limits/usage-tiers?context=tier-one). In **tier-1** for GPT-4, TPM is 10,000. You might be using different tier than tier-1 e.g. free, tier-2 etc. where TPM values are different.

What are **tokens** and how to **count** them you can find [here](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them) .


### \# pattern's list

[analyze_requests_init](./patterns/analyze_requests_init/README.md)  
[analyze_html](./patterns/analyze_html/README.md)


### \# example usage

#### > analyze and display review
```
./gofileai \
-f /tmp/input.log \
-m "please list all uniq Request lines in one section and uniq User-Agent lines in separate section."
```

#### > analyze and save review to a file
```
./gofileai \
-f /tmp/input.log \
-m "please list all uniq Request lines in one section and uniq User-Agent lines in separate section." \
-o
```

#### > analyze based on defined pattern and save review to a file

Patterns can be find [here](./patterns/) .

```
./gofileai \
-f /tmp/input.log \
-p analyze_requests_init \
-o
```

### RAG [work in progress]

Very simple RAG implementation. RAG files (.txt based) are [here](./ragdata/).

```
> without 'rag'

$ ./gofileai \
-f /tmp/input.log \
-m "tell me more about michalswi in one sentence"
OpenAI review started..
ChatGPT's review base on `/tmp/input.log` file:
As of my knowledge cutoff in October 2023, there isn't widely available information about "michalswi." Could you provide more context or specify where you encountered this name?
```

```
> with 'rag'

$ ./gofileai \
-f /tmp/input.log \
-m "tell me more about michalswi in one sentence" \
--rag
OpenAI review started..
ChatGPT's review base on `/tmp/input.log` file:
Michalswi is a GitHub repository owner and developer known for managing multiple repositories and contributing to various open-source projects, which can be explored on his [GitHub profile](https://github.com/michalswi).
```
