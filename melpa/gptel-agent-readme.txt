Agentic use mode for gptel: This is a collection of tools and prompts for
gptel to enable easy agentic LLM use.

How to use:

- Set `gptel-model' and `gptel-backend' to use your preferred LLM.

- Run M-x `gptel-agent'.  This will open a buffer with the agent preset
  loaded.

- Use gptel as usual, calling `gptel-send' etc.

- If you change the system prompt, tools or other settings in this buffer, you
  can reset the agent state by (re)applying the "gptel-agent" preset from
  gptel's menu.

- As with gptel, you can use gptel-agent in any buffer.  Just apply the
  "gptel-agent" preset in the buffer.

gptel-agent can delegate tasks to "sub-agents".  Sub-agents can be specified
in Markdown or Org files in a directory.  To see how to specify agents,
examine the "agents" directory in this package.  You can add your directory
of agents to `gptel-agent-dirs', which see.

Please note: gptel-agent uses significantly more tokens than the average
gptel LLM interaction.
