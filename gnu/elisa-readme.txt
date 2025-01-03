1 ELISA
═══════

  [file:https://img.shields.io/badge/license-GPL_3-green.svg]
  [file:https://melpa.org/packages/elisa-badge.svg]
  [file:https://stable.melpa.org/packages/elisa-badge.svg]
  [https://elpa.gnu.org/packages/elisa.svg]

  ELISA (Emacs Lisp Information System Assistant) is a system designed
  to provide informative answers to user queries by leveraging a
  Retrieval Augmented Generation (RAG) approach.


[file:https://img.shields.io/badge/license-GPL_3-green.svg]
<http://www.gnu.org/licenses/gpl-3.0.txt>

[file:https://melpa.org/packages/elisa-badge.svg]
<https://melpa.org/#/elisa>

[file:https://stable.melpa.org/packages/elisa-badge.svg]
<https://stable.melpa.org/#/elisa>

[https://elpa.gnu.org/packages/elisa.svg]
<https://elpa.gnu.org/packages/elisa.html>

1.0.1 Data Sources and Processing
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  ELISA can access and process information from multiple sources,
  including:

  ⁃ *Local Files:* ELISA can analyze text content within local files,
    enabling it to retrieve information specific to a user's projects or
    documents.
  ⁃ *Info manuals:* ELISA has access to the comprehensive Emacs info
    manuals covering Emacs itself, Emacs Lisp, and various Emacs Lisp
    packages.
  ⁃ *Web Search:* ELISA integrates web search capabilities to provide
    access to a vast pool of publicly available information.


1.0.2 RAG Methodology
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  ELISA implements a RAG framework to process and respond to
  queries. This involves:

  1. *Data Parsing:* Input data is parsed and organized into structured
     collections for efficient retrieval.
  2. *Contextual Analysis:* When a query is received, ELISA analyzes the
     context within relevant data collections to identify passages
     containing potentially useful information.
  3. *Response Generation:* ELISA synthesizes a response based on the
     identified contextual quotes, aiming to provide a comprehensive and
     accurate answer to the user's question.


1.0.3 Collections
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  ELISA operates on collections. Collection has name and contains quotes
  from parsed documents. Every web search, every directory parsing
  creates new collection. ELISA searches in enabled collection, adds
  relevant information to context and let LLM answer user query.


1.0.4 PDF files support
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  For pdf, doc and other complex documents support used [Apache Tika].


[Apache Tika] <https://tika.apache.org>


1.1 Installation
────────────────

  You need emacs 29.2 or newer to use this package.

  This package now on [MELPA] and you can just `M-x' `package-install'
  `elisa'.


[MELPA] <https://melpa.org/#/getting-started>

1.1.1 System dependencies
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

◊ 1.1.1.1 Sqlite extensions

  Then you need to download `sqlite-vss'. You can do it manually from
  <https://github.com/asg017/sqlite-vss/releases> or by calling `M-x'
  `elisa-download-sqlite-vss'.


◊ 1.1.1.2 Large language models

  You can use this package with different llm providers. By default it
  uses [ollama] provider both for embeddings and chat. If you ok with
  it, you need to install [ollama] and pull used models:

  ┌────
  │ ollama pull nomic-embed-text
  │ ollama pull sskostyaev/openchat:8k-rag
  └────

  Second model is just [openchat] with exactly 2 tweaks: context window
  extended to 8k and temperature set to 0 to better usage for RAG
  (Retrieval Augmented Generation). You can try other models, for
  example:
  • [all-minilm] for embeddings
  • for chat
  • other [models] or [providers]
  • [create your own model]

  I prefer this models:

  ┌────
  │ ollama pull gemma2:9b-instruct-q6_K
  │ ollama pull qwen2.5:3b
  │ ollama pull snowflake-arctic-embed2
  └────


  [ollama] <https://github.com/jmorganca/ollama>

  [openchat] <https://ollama.com/library/openchat>

  [all-minilm] <https://ollama.com/library/all-minilm>

  [models] <https://ollama.com/library>

  [providers]
  <https://github.com/ahyatt/llm?tab=readme-ov-file#setting-up-providers>

  [create your own model]
  <https://github.com/ollama/ollama?tab=readme-ov-file#create-a-model>


◊ 1.1.1.3 Complex documents

  For pdf, doc etc. you need to run [Apache Tika] service locally. You
  can do it using docker:

  ┌────
  │ docker run -d -p 127.0.0.1:9998:9998 apache/tika:latest-full
  └────


  [Apache Tika] <https://tika.apache.org>


◊ 1.1.1.4 Reranker

  Reranker disabled by default to decrease number of system
  dependencies, but it improves quality of retrieving and answers
  significantly. You can find installation instructions [here].
  Recommended.


  [here] <https://github.com/s-kostyaev/reranker>


◊ 1.1.1.5 Web search provider

  By defauld [duckduckgo] used for web search. But I prefer
  [searxng]. The simplest way to use searxng is [docker]. You need to
  enable json format in [settings].


  [duckduckgo] <https://duckduckgo.com>

  [searxng] <https://github.com/searxng/searxng>

  [docker] <https://github.com/searxng/searxng-docker>

  [settings]
  <https://docs.searxng.org/admin/settings/settings_search.html#settings-search>


◊ 1.1.1.6 Parse info manuals

  Create index for builtin, external or all info manuals by one of this
  commands:
  • `elisa-async-parse-builtin-manuals'
  • `elisa-async-parse-external-manuals'
  • `elisa-async-parse-all-manuals'

  This can take some time.


1.2 Commands
────────────

1.2.1 elisa-chat
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Entrypoint. Makes hybrid search in enabled collections, add founded
  quotes into context and query llm for prompt. Uses `ellama' under the
  hood.


1.2.2 elisa-download-sqlite-vss
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Download [sqlite vss] extension to provide similarity search.


[sqlite vss] <https://github.com/asg017/sqlite-vss>


1.2.3 elisa-async-parse-builtin-manuals
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Parse builtin emacs info manuals asyncronously. Can take long time.


1.2.4 elisa-async-parse-external-manuals
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Parse external emacs info manuals asyncronously. Can take long time.


1.2.5 elisa-async-parse-all-manuals
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Parse all emacs info manuals asyncronously.

  One of parse functions should be called before `elisa-chat' to create
  index.


1.2.6 elisa-web-search
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Search the web and answer to user query based on found information.


◊ 1.2.6.1 How it works

  Search the web for user query. Create new collection with user query
  as name. Parse web pages to this new collection. Search in this
  collection. Add related information to context. Ask llm to answer user
  query based on provided context.


1.2.7 elisa-async-parse-directory
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Parse directory as new collection. Can take long time. Works
  asyncronously and incrementally.


1.2.8 elisa-reparse-current-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Incrementally reparse current directory collection.  It does nothing
  if buffer file not inside one of existing collections.  Works
  asyncronously.


1.2.9 elisa-create-empty-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Create new empty collection.


1.2.10 elisa-add-file-to-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add file to collection.


1.2.11 elisa-add-webpage-to-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add webpage to collection.


1.2.12 elisa-enable-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Enable collection for `elisa-chat'.


1.2.13 elisa-enable-all-collections
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Enable all collections.


1.2.14 elisa-disable-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Disable collection.


1.2.15 elisa-disble-all-collections
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Disable all collections.


1.2.16 elisa-remove-collection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Removes collection and all its data from index.


1.2.17 elisa-async-recalculate-embeddings
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Recalculate embeddings asynchronously. Use it after changing
  `elisa-embeddings-provider' variable. Can take some time. Works faster
  with `elisa-batch-embeddings-enabled'.


1.3 Configuration
─────────────────

  Example configuration.

  ┌────
  │ (use-package elisa
  │   :init
  │   (setopt elisa-limit 5)
  │   ;; reranker increases answer quality significantly
  │   (setopt elisa-reranker-enabled t)
  │   ;; prompt rewriting may increase quality of answers
  │   ;; disable it if you want direct control over prompt
  │   (setopt elisa-prompt-rewriting-enabled t)
  │   (require 'llm-ollama)
  │   ;; gemma 2 works very good in my use cases
  │   ;; it also boasts strong multilingual capabilities
  │   ;; (setopt elisa-chat-provider
  │   ;; 	  (make-llm-ollama
  │   ;; 	   :chat-model "gemma2:9b-instruct-q6_K"
  │   ;; 	   :embedding-model "snowflake-arctic-embed2"
  │   ;; 	   ;; set context window to 8k
  │   ;; 	   :default-chat-non-standard-params '(("num_ctx" . 8192))))
  │   ;;
  │   ;; qwen 2.5 3b works good in my test cases and provide longer context
  │   (setopt elisa-chat-provider
  │ 	(make-llm-ollama
  │ 	 :chat-model "qwen2.5:3b"
  │ 	 :embedding-model "snowflake-arctic-embed2"
  │ 	 :default-chat-temperature 0.1
  │ 	 :default-chat-non-standard-params '(("num_ctx" . 32768))))
  │   ;; this embedding model has stong multilingual capabilities
  │   (setopt elisa-embeddings-provider (make-llm-ollama :embedding-model "snowflake-arctic-embed2"))
  │   ;; enable batch embeddings for faster processing
  │   (setopt elisa-batch-embeddings-enabled t)
  │   :config
  │   ;; searxng works better than duckduckgo in my tests
  │   (setopt elisa-web-search-function 'elisa-search-searxng))
  └────


1.3.1 ELISA Custom Variables
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

◊ 1.3.1.1 General Settings

  ⁃ `elisa-embeddings-provider':
    ‣ Description: LLM provider to generate embeddings for text.
    ‣ Default: `(make-llm-ollama :embedding-model "nomic-embed-text")'

  ⁃ `elisa-chat-provider':
    ‣ Description: LLM provider used for chat interactions.
    ‣ Default: `(make-llm-ollama :chat-model
            "sskostyaev/openchat:8k-rag" :embedding-model
            "nomic-embed-text")'

  ⁃ `elisa-db-directory':
    ‣ Type: Directory
    ‣ Description: Specifies the directory where ELISA stores its
      database.
    ‣ Default: `(file-name-concat user-emacs-directory "elisa")' (within
      your Emacs config directory)

  ⁃ `elisa-limit':
    ‣ Type: Integer
    ‣ Description: Controls the number of quotes passed to the LLM
      context for generating an answer.
    ‣ Default: 5

  ⁃ `elisa-find-executable':
    ‣ Type: String
    ‣ Description: Path to the `find' command executable. Used for
      locating files.
    ‣ Default: "find"


◊ 1.3.1.2 File System and Database Management

  ⁃ `elisa-tar-executable':
    ‣ Type: String
    ‣ Description: Path to the `tar' command executable. Used for
      archiving files.
    ‣ Default: "tar"

  ⁃ `elisa-sqlite-vss-version':
    ‣ Type: String
    ‣ Description: Version of the SQLite VSS extension.

  ⁃ `elisa-sqlite-vss-path':
    ‣ Type: File path
    ‣ Description: Path to the SQLite VSS extension file.

  ⁃ `elisa-sqlite-vector-path':
    ‣ Type: File path
    ‣ Description: Path to the SQLite Vector extension file.


◊ 1.3.1.3 Text Processing and Semantic Splitting

  ⁃ `elisa-semantic-split-function':
    ‣ Type: Function
    ‣ Description: Function used to split text into semantically
      meaningful chunks.
    ‣ Default: `elisa-split-by-paragraph'

  ⁃ `elisa-prompt-rewriting-enabled':
    ‣ Type: Boolean
    ‣ Description: Enables or disables prompt rewriting for better
      retrieving.
    ‣ Default: `t' (enabled)

  ⁃ `elisa-chat-prompt-template':
    ‣ Type: String
    ‣ Description: Template used for constructing the chat prompt.

  ⁃ `elisa-rewrite-prompt-template':
    ‣ Type: String
    ‣ Description: Template used for rewriting prompts for better
      retrieval.

  ⁃ `elisa-batch-embeddings-enabled':
    ‣ Type: Boolean
    ‣ Description: Enable batch embeddings if supported.

  ⁃ `elisa-batch-size':
    ‣ Type: Integer
    ‣ Description: Batch size to send to provider during batch
      embeddings calculation.
    ‣ Default: 300


◊ 1.3.1.4 Web Search and Integration

  ⁃ `elisa-searxng-url':
    ‣ Type: String
    ‣ Description: URL of your SearXNG instance.
    ‣ Default: "<http://localhost:8080/>"

  ⁃ `elisa-pandoc-executable':
    ‣ Type: String
    ‣ Description: Path to the `pandoc' command executable. Used for
      converting documents to text.
    ‣ Default: "pandoc"

  ⁃ `elisa-webpage-extraction-function':
    ‣ Type: Function
    ‣ Description: Function used to extract the content from a webpage.
    ‣ Default: `elisa-get-webpage-buffer'

  ⁃ `elisa-web-search-function':
    ‣ Type: Function
    ‣ Description: Function responsible for performing web searches
      using the provided prompt.
    ‣ Default: `elisa-search-duckduckgo'

  ⁃ `elisa-web-pages-limit':
    ‣ Type: Integer
    ‣ Description: Maximum number of web pages to parse during a search.
    ‣ Default: 10


◊ 1.3.1.5 Reranking

  ⁃ `elisa-breakpoint-threshold-amount':
    ‣ Type: Float
    ‣ Description: Threshold used for controlling the granularity of
      semantic splitting.
    ‣ Default: 0.4

  ⁃ `elisa-reranker-enabled':
    ‣ Type: Boolean
    ‣ Description: Enables or disables reranking, which can improve
      retrieval quality by ranking retrieved quotes based on relevance.
    ‣ Default: `nil' (not set)

  ⁃ `elisa-reranker-url':
    ‣ Type: String
    ‣ Description: URL of the reranking service.
    ‣ Default: "<http://127.0.0.1:8787/>"

  ⁃ `elisa-reranker-similarity-threshold':
    ‣ Type: Float
    ‣ Description: Similarity threshold for reranking. Quotes below this
      threshold will be filtered out. If not set all `ellama-limit'
      quotes will be added to context.
    ‣ Default: 0

  ⁃ `elisa-reranker-limit':
    ‣ Type: Integer
    ‣ Description: Number of quotes to send to the reranker.
    ‣ Default: 20


◊ 1.3.1.6 File Parsing and Exclusion

  ⁃ `elisa-ignore-patterns-files':
    ‣ Type: List of strings
    ‣ Description: List of file name patterns (e.g., `.gitignore') used
      to ignore files during parsing.
    ‣ Default: `(".gitignore" ".ignore" ".rgignore")'

  ⁃ `elisa-ignore-invisible-files':
    ‣ Type: Boolean
    ‣ Description: Toggles whether invisible files and directories
      should be ignored during parsing.
    ‣ Default: `t' (true)

  ⁃ `elisa-tika-url':
    ‣ Type: String
    ‣ Description: Apache tika url for file parsing.
    ‣ Default: `"http://localhost:9998/"'

  ⁃ `elisa-complex-file-extraction-function':
    ‣ Type: Function
    ‣ Description: Function to get buffer with complex file (like pdf,
      odt etc.) content.
    ‣ Default: `#'elisa-parse-with-tika-buffer'

  ⁃ `elisa-supported-complex-document-extensions':
    ‣ Type: List of strings
    ‣ Description: Supported complex document file extensions.
    ‣ Default: `'("doc" "dot" "ppt" "xls" "rtf" "docx" "pptx" "xlsx"
      "xlsm" "pdf" "epub" "msg" "odt" "odp" "ods" "odg" "docm")'


◊ 1.3.1.7 ELISA Chat Collections

  ⁃ `elisa-enabled-collections':
    ‣ Type: List of strings
    ‣ Description: Specifies which collections are enabled for chat
      interactions.
    ‣ Default: `("builtin manuals" "external manuals")'


1.4 Contributions
─────────────────

  To contribute, submit a pull request or report a bug. This library is
  planned to be part of GNU ELPA; major contributions must be from
  someone with FSF papers. Alternatively, you can write a module and
  share it on a different archive like MELPA.
