1 Ellama
════════

  [file:https://img.shields.io/badge/license-GPL_3-green.svg]
  [file:https://melpa.org/packages/ellama-badge.svg]
  [file:https://stable.melpa.org/packages/ellama-badge.svg]
  [file:https://elpa.gnu.org/packages/ellama.svg]

  Ellama is a tool for interacting with large language models from
  Emacs. It allows you to ask questions and receive responses from the
  LLMs. Ellama can perform various tasks such as translation, code
  review, summarization, enhancing grammar/spelling or wording and more
  through the Emacs interface. Ellama natively supports streaming
  output, making it effortless to use with your preferred text editor.

  The name "ellama" is derived from "Emacs Large LAnguage Model
  Assistant". Previous sentence was written by Ellama itself.
  <file:imgs/reasoning-models.gif>


[file:https://img.shields.io/badge/license-GPL_3-green.svg]
<http://www.gnu.org/licenses/gpl-3.0.txt>

[file:https://melpa.org/packages/ellama-badge.svg]
<https://melpa.org/#/ellama>

[file:https://stable.melpa.org/packages/ellama-badge.svg]
<https://stable.melpa.org/#/ellama>

[file:https://elpa.gnu.org/packages/ellama.svg]
<https://elpa.gnu.org/packages/ellama.html>

1.1 Installation
────────────────

  Just `M-x' `package-install' Enter `ellama' Enter. By default it uses
  [ollama] provider. If you ok with it, you need to install [ollama] and
  pull [any ollama model] like this:

  ┌────
  │ ollama pull qwen2.5:3b
  └────

  You can use `ellama' with other model or other llm provider.  Without
  any configuration, the first available ollama model will be used.  You
  can customize ellama configuration like this:

  ┌────
  │ (use-package ellama
  │   :ensure t
  │   :bind ("C-c e" . ellama-transient-main-menu)
  │   ;; send last message in chat buffer with C-c C-c
  │   :hook (org-ctrl-c-ctrl-c-final . ellama-chat-send-last-message)
  │   :init (setopt ellama-auto-scroll t)
  │   :config
  │   ;; show ellama context in header line in all buffers
  │   (ellama-context-header-line-global-mode +1))
  └────

  More sofisticated configuration example:

  ┌────
  │ (use-package ellama
  │   :ensure t
  │   :bind ("C-c e" . ellama-transient-main-menu)
  │   ;; send last message in chat buffer with C-c C-c
  │   :hook (org-ctrl-c-ctrl-c-final . ellama-chat-send-last-message)
  │   :init
  │   ;; setup key bindings
  │   ;; (setopt ellama-keymap-prefix "C-c e")
  │   ;; language you want ellama to translate to
  │   (setopt ellama-language "German")
  │   ;; could be llm-openai for example
  │   (require 'llm-ollama)
  │   (setopt ellama-provider
  │ 	(make-llm-ollama
  │ 	 ;; this model should be pulled to use it
  │ 	 ;; value should be the same as you print in terminal during pull
  │ 	 :chat-model "llama3:8b-instruct-q8_0"
  │ 	 :embedding-model "nomic-embed-text"
  │ 	 :default-chat-non-standard-params '(("num_ctx" . 8192))))
  │   (setopt ellama-summarization-provider
  │ 	(make-llm-ollama
  │ 	 :chat-model "qwen2.5:3b"
  │ 	 :embedding-model "nomic-embed-text"
  │ 	 :default-chat-non-standard-params '(("num_ctx" . 32768))))
  │   (setopt ellama-coding-provider
  │ 	(make-llm-ollama
  │ 	 :chat-model "qwen2.5-coder:3b"
  │ 	 :embedding-model "nomic-embed-text"
  │ 	 :default-chat-non-standard-params '(("num_ctx" . 32768))))
  │   ;; Predefined llm providers for interactive switching.
  │   ;; You shouldn't add ollama providers here - it can be selected interactively
  │   ;; without it. It is just example.
  │   (setopt ellama-providers
  │ 	'(("zephyr" . (make-llm-ollama
  │ 		       :chat-model "zephyr:7b-beta-q6_K"
  │ 		       :embedding-model "zephyr:7b-beta-q6_K"))
  │ 	  ("mistral" . (make-llm-ollama
  │ 			:chat-model "mistral:7b-instruct-v0.2-q6_K"
  │ 			:embedding-model "mistral:7b-instruct-v0.2-q6_K"))
  │ 	  ("mixtral" . (make-llm-ollama
  │ 			:chat-model "mixtral:8x7b-instruct-v0.1-q3_K_M-4k"
  │ 			:embedding-model "mixtral:8x7b-instruct-v0.1-q3_K_M-4k"))))
  │   ;; Naming new sessions with llm
  │   (setopt ellama-naming-provider
  │ 	(make-llm-ollama
  │ 	 :chat-model "llama3:8b-instruct-q8_0"
  │ 	 :embedding-model "nomic-embed-text"
  │ 	 :default-chat-non-standard-params '(("stop" . ("\n")))))
  │   (setopt ellama-naming-scheme 'ellama-generate-name-by-llm)
  │   ;; Translation llm provider
  │   (setopt ellama-translation-provider
  │ 	(make-llm-ollama
  │ 	 :chat-model "qwen2.5:3b"
  │ 	 :embedding-model "nomic-embed-text"
  │ 	 :default-chat-non-standard-params
  │ 	 '(("num_ctx" . 32768))))
  │   (setopt ellama-extraction-provider (make-llm-ollama
  │ 				    :chat-model "qwen2.5-coder:7b-instruct-q8_0"
  │ 				    :embedding-model "nomic-embed-text"
  │ 				    :default-chat-non-standard-params
  │ 				    '(("num_ctx" . 32768))))
  │   ;; customize display buffer behaviour
  │   ;; see ~(info "(elisp) Buffer Display Action Functions")~
  │   (setopt ellama-chat-display-action-function #'display-buffer-full-frame)
  │   (setopt ellama-instant-display-action-function #'display-buffer-at-bottom)
  │   :config
  │   ;; show ellama context in header line in all buffers
  │   (ellama-context-header-line-global-mode +1))
  └────


[ollama] <https://github.com/jmorganca/ollama>

[any ollama model] <https://ollama.com/models>


1.2 Commands
────────────

1.2.1 ellama-chat
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Ask Ellama about something by entering a prompt in an interactive
  buffer and continue conversation. If called with universal argument
  (`C-u') will start new session with llm model interactive selection.


1.2.2 ellama-write
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  This command allows you to generate text using an LLM. When called
  interactively, it prompts for an instruction that is then used to
  generate text based on the context. If a region is active, the
  selected text is added to the context before generating the response.


1.2.3 ellama-chat-send-last-message
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Send last user message extracted from current ellama chat buffer.


1.2.4 ellama-ask-about
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Ask Ellama about a selected region or the current buffer.


1.2.5 ellama-ask-selection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Send selected region or current buffer to ellama chat.


1.2.6 ellama-ask-line
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Send current line to ellama chat.


1.2.7 ellama-complete
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Complete text in current buffer with ellama.


1.2.8 ellama-translate
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Ask Ellama to translate a selected region or word at the point.


1.2.9 ellama-translate-buffer
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Translate current buffer.


1.2.10 ellama-define-word
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Find the definition of the current word using Ellama.


1.2.11 ellama-summarize
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Summarize a selected region or the current buffer using Ellama.


1.2.12 ellama-summarize-killring
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Summarize text from the kill ring.


1.2.13 ellama-code-review
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Review code in a selected region or the current buffer using Ellama.


1.2.14 ellama-change
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Change text in a selected region or the current buffer according to a
  provided change.


1.2.15 ellama-make-list
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Create a markdown list from the active region or the current buffer
  using Ellama.


1.2.16 ellama-make-table
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Create a markdown table from the active region or the current buffer
  using Ellama.


1.2.17 ellama-summarize-webpage
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Summarize a webpage fetched from a URL using Ellama.


1.2.18 ellama-provider-select
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Select ellama provider.


1.2.19 ellama-code-complete
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Complete selected code or code in the current buffer according to a
  provided change using Ellama.


1.2.20 ellama-code-add
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Generate and insert new code based on description. This function
  prompts the user to describe the code they want to generate. If a
  region is active, it includes the selected text as context for code
  generation.  <file:imgs/writing-code.gif>


1.2.21 ellama-code-edit
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Change selected code or code in the current buffer according to a
  provided change using Ellama.


1.2.22 ellama-code-improve
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Change selected code or code in the current buffer according to a
  provided change using Ellama.


1.2.23 ellama-generate-commit-message
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Generate commit message based on diff.


1.2.24 ellama-proofread
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Proofread selected text.


1.2.25 ellama-improve-wording
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Enhance the wording in the currently selected region or buffer using
  Ellama.


1.2.26 ellama-improve-grammar
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Enhance the grammar and spelling in the currently selected region or
  buffer using Ellama.


1.2.27 ellama-improve-conciseness
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Make the text of the currently selected region or buffer concise and
  simple using Ellama.


1.2.28 ellama-make-format
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Render the currently selected text or the text in the current buffer
  as a specified format using Ellama.


1.2.29 ellama-load-session
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Load ellama session from file.


1.2.30 ellama-session-delete
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Delete ellama session.


1.2.31 ellama-session-switch
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Change current active session.


1.2.32 ellama-session-kill
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Select and kill one of active sessions.


1.2.33 ellama-session-rename
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Rename current ellama session.


1.2.34 ellama-context-add-file
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add file to context.


1.2.35 ellama-context-add-directory
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add all files in directory to the context.


1.2.36 ellama-context-add-buffer
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add buffer to context.


1.2.37 ellama-context-add-selection
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add selected region to context.


1.2.38 ellama-context-add-info-node
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Add info node to context.


1.2.39 ellama-context-reset
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Clear global context.


1.2.40 ellama-manage-context
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Manage the global context. Inside context management buffer you can
  see ellama context elements. Availible actions with key bindings:
  • *`n'*: Move to the next line.
  • *`p'*: Move to the previous line.
  • *`q'*: Quit the window.
  • *`g'*: Update context management buffer.
  • *`a'*: Open the transient context menu for adding new elements.
  • *`d'*: Remove the context element at the current point.
  • *`RET'*: Preview the context element at the current point.


1.2.41 ellama-preview-context-element-at-point
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Preview ellama context element at point. Works inside ellama context
  management buffer.


1.2.42 ellama-remove-context-element-at-point
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Remove ellama context element at point from global context. Works
  inside ellama context management buffer.


1.2.43 ellama-chat-translation-enable
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Chat translation enable.


1.2.44 ellama-chat-translation-disable
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Chat translation disable.


1.2.45 ellama-solve-reasoning-problem
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Solve reasoning problem with [Absctraction of Thought] technique. It
  uses a chain of multiple messages to LLM and help it to provide much
  better answers on reasoning problems. Even small LLMs like [phi3-mini]
  provides much better results on reasoning tasks using AoT.


[Absctraction of Thought] <https://arxiv.org/pdf/2406.12442>

[phi3-mini] <https://ollama.com/library/phi3>


1.2.46 ellama-solve-domain-specific-problem
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Solve domain specific problem with simple chain. It makes LLMs act
  like a professional and adds a planning step.


1.2.47 ellama-community-prompts-select-blueprint
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Select a prompt from the community prompt collection.  The user is
  prompted to choose a role, and then a corresponding prompt is inserted
  into a blueprint buffer.


1.2.48 ellama-community-prompts-update-variables
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Prompt user for values of variables found in current buffer and update
  them.


1.3 Keymap
──────────

  In any buffer where there is active ellama streaming, you can press
  `C-g' and it will cancel current stream.

  Here is a table of keybindings and their associated functions in
  Ellama, using the `ellama-keymap-prefix' prefix (not set by default):

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   Keymap  Function                         Description                  
  ───────────────────────────────────────────────────────────────────────
   "w"     ellama-write                     Write                        
   "c c"   ellama-code-complete             Code complete                
   "c a"   ellama-code-add                  Code add                     
   "c e"   ellama-code-edit                 Code edit                    
   "c i"   ellama-code-improve              Code improve                 
   "c r"   ellama-code-review               Code review                  
   "c m"   ellama-generate-commit-message   Generate commit message      
   "s s"   ellama-summarize                 Summarize                    
   "s w"   ellama-summarize-webpage         Summarize webpage            
   "s c"   ellama-summarize-killring        Summarize killring           
   "s l"   ellama-load-session              Session Load                 
   "s r"   ellama-session-rename            Session rename               
   "s d"   ellama-session-delete            Delete delete                
   "s a"   ellama-session-switch            Session activate             
   "P"     ellama-proofread                 Proofread                    
   "i w"   ellama-improve-wording           Improve wording              
   "i g"   ellama-improve-grammar           Improve grammar and spelling 
   "i c"   ellama-improve-conciseness       Improve conciseness          
   "m l"   ellama-make-list                 Make list                    
   "m t"   ellama-make-table                Make table                   
   "m f"   ellama-make-format               Make format                  
   "a a"   ellama-ask-about                 Ask about                    
   "a i"   ellama-chat                      Chat (ask interactively)     
   "a l"   ellama-ask-line                  Ask current line             
   "a s"   ellama-ask-selection             Ask selection                
   "t t"   ellama-translate                 Text translate               
   "t b"   ellama-translate-buffer          Translate buffer             
   "t e"   ellama-chat-translation-enable   Translation enable           
   "t d"   ellama-chat-translation-disable  Translation disable          
   "t c"   ellama-complete                  Text complete                
   "d w"   ellama-define-word               Define word                  
   "x b"   ellama-context-add-buffer        Context add buffer           
   "x f"   ellama-context-add-file          Context add file             
   "x d"   ellama-context-add-directory     Context add directory        
   "x s"   ellama-context-add-selection     Context add selection        
   "x i"   ellama-context-add-info-node     Context add info node        
   "x r"   ellama-context-reset             Context reset                
   "p s"   ellama-provider-select           Provider select              
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━


1.4 Configuration
─────────────────

  The following variables can be customized for the Ellama client:

  • `ellama-enable-keymap': Enable the Ellama keymap.
  • `ellama-keymap-prefix': The keymap prefix for Ellama.
  • `ellama-user-nick': The user nick in logs.
  • `ellama-assistant-nick': The assistant nick in logs.
  • `ellama-language': The language for Ollama translation. Default
  language is english.
  • `ellama-provider': llm provider for ellama.
  There are many supported providers: `ollama', `open ai', `vertex',
  `GPT4All'. For more information see [llm documentation].
  • `ellama-providers': association list of model llm providers with
    name as key.
  • `ellama-spinner-enabled': Enable spinner during text generation.
  • `ellama-spinner-type': Spinner type for ellama. Default type is
  `progress-bar'.
  • `ellama-ollama-binary': Path to ollama binary.
  • `ellama-auto-scroll': If enabled ellama buffer will scroll
    automatically during generation. Disabled by default.
  • `ellama-fill-paragraphs': Option to customize ellama paragraphs
    filling behaviour.
  • `ellama-name-prompt-words-count': Count of words in prompt to
    generate name.
  • Prompt templates for every command.
  • `ellama-chat-done-callback': Callback that will be called on ellama
  chat response generation done. It should be a function with single
  argument generated text string.
  • `ellama-nick-prefix-depth': User and assistant nick prefix depth.
    Default value is 2.
  • `ellama-sessions-directory': Directory for saved ellama sessions.
  • `ellama-major-mode': Major mode for ellama commands. Org mode by
    default.
  • `ellama-session-auto-save': Automatically save ellama sessions if
    set. Enabled by default.
  • `ellama-naming-scheme': How to name new sessions.
  • `ellama-naming-provider': LLM provider for generating session names
    by LLM. If not set `ellama-provider' will be used.
  • `ellama-chat-translation-enabled': Enable chat translations if set.
  • `ellama-translation-provider': LLM translation provider.
    `ellama-provider' will be used if not set.
  • `ellama-coding-provider': LLM coding tasks provider.
    `ellama-provider' will be used if not set.
  • `ellama-summarization-provider' LLM summarization provider.
    `ellama-provider' will be used if not set.
  • `ellama-show-quotes': Show quotes content in chat buffer. Disabled
    by default.
  • `ellama-chat-display-action-function': Display action function for
    `ellama-chat'.
  • `ellama-instant-display-action-function': Display action function
    for `ellama-instant'.
  • `ellama-translate-italic': Translate italic during markdown to org
    transformations. Enabled by default.
  • `ellama-extraction-provider': LLM provider for data extraction.
  • `ellama-text-display-limit': Limit for text display in context
    elements.
  • `ellama-context-poshandler': Position handler for displaying context
    buffer.  `posframe-poshandler-frame-top-center' will be used if not
    set.
  • `ellama-context-border-width': Border width for the context buffer.
  • `ellama-session-remove-reasoning': Remove internal reasoning from
    the session after ellama provide an answer. This can improve
    long-term communication with reasoning models. Enabled by default.
  • `ellama-session-hide-org-quotes': Hide org quotes in the Ellama
    session buffer. From now on, think tags will be replaced with quote
    blocks. If this flag is enabled, reasoning steps will be collapsed
    after generation and upon session loading. Enabled by default.
  • `ellama-output-remove-reasoning': Eliminate internal reasoning from
    ellama output to enhance the versatility of reasoning models across
    diverse applications.
  • `ellama-context-posframe-enabled': Enable showing posframe with
    ellama context.
  • `ellama-manage-context-display-action-function': Display action
    function for `ellama-render-context'. Default value
    `display-buffer-same-window'.
  • `ellama-preview-context-element-display-action-function': Display
    action function for `ellama-preview-context-element'.
  • `ellama-context-line-always-visible': Make context header or mode
    line always visible, even with empty context.
  • `ellama-community-prompts-url': The URL of the community prompts
    collection.
  • `ellama-community-prompts-file': Path to the CSV file containing
    community prompts.  This file is expected to be located inside an
    `ellama' subdirectory within your `user-emacs-directory'.


[llm documentation] <https://elpa.gnu.org/packages/llm.html>


1.5 Minor modes
───────────────

1.5.1 ellama-context-header-line-mode
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  *Description:* Toggle the Ellama Context header line mode. This minor
  mode updates the header line to display context-specific information.

  *Usage:* To enable or disable `ellama-context-header-line-mode', use
  the command:

  M-x ellama-context-header-line-mode

  When enabled, this mode adds a hook to `window-state-change-hook' to
  update the header line whenever the window state changes. It also
  calls `ellama-context-update-header-line' to initialize the header
  line with context-specific information.

  When disabled, it removes the evaluation of `(:eval
  (ellama-context-line))' from `header-line-format'.


1.5.2 ellama-context-header-line-global-mode
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  *Description:* Globalized version of
  `ellama-context-header-line-mode'. This mode ensures that
  `ellama-context-header-line-mode' is enabled in all buffers.

  *Usage:* To enable or disable
  `ellama-context-header-line-global-mode', use the command:

  M-x ellama-context-header-line-global-mode

  This globalized minor mode provides a convenient way to ensure that
  context-specific header line information is always available,
  regardless of the buffer being edited.


1.5.3 ellama-context-mode-line-mode
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  *Description:* Toggle the Ellama Context mode line mode. This minor
  mode updates the mode line to display context-specific information.

  *Usage:* To enable or disable `ellama-context-mode-line-mode', use the
  command:

  M-x ellama-context-mode-line-mode

  When enabled, this mode adds a hook to `window-state-change-hook' to
  update the mode line whenever the window state changes. It also calls
  `ellama-context-update-mode-line' to initialize the mode line with
  context-specific information.

  When disabled, it removes the evaluation of `(:eval
  (ellama-context-line))' from `mode-line-format'.


1.5.4 ellama-context-mode-line-global-mode
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  *Description:* Globalized version of
  `ellama-context-mode-line-mode'. This mode ensures that
  `ellama-context-mode-line-mode' is enabled in all buffers.

  *Usage:* To enable or disable `ellama-context-mode-line-global-mode',
  use the command:

  M-x ellama-context-mode-line-global-mode

  This globalized minor mode provides a convenient way to ensure that
  context-specific mode line information is always available, regardless
  of the buffer being edited.


1.6 Acknowledgments
───────────────────

  Thanks [Jeffrey Morgan] for excellent project [ollama]. This project
  cannot exist without it.

  Thanks [zweifisch] - I got some ideas from [ollama.el] what ollama
  client in Emacs can do.

  Thanks [Dr. David A. Kunz] - I got more ideas from [gen.nvim].

  Thanks [Andrew Hyatt] for `llm' library. Without it only `ollama'
  would be supported.


[Jeffrey Morgan] <https://github.com/jmorganca>

[ollama] <https://github.com/jmorganca/ollama>

[zweifisch] <https://github.com/zweifisch>

[ollama.el] <https://github.com/zweifisch/ollama>

[Dr. David A. Kunz] <https://github.com/David-Kunz>

[gen.nvim] <https://github.com/David-Kunz/gen.nvim>

[Andrew Hyatt] <https://github.com/ahyatt>


2 Contributions
═══════════════

  To contribute, submit a pull request or report a bug. This library is
  part of GNU ELPA; major contributions must be from someone with FSF
  papers. Alternatively, you can write a module and share it on a
  different archive like MELPA.
