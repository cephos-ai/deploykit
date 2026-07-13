# workflows

n8n workflows. Need a running n8n first:

```bash
docker run -it --rm -p 5678:5678 -v ~/.n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

Open http://localhost:5678, then import each `workflow.json` (Workflows → Import from File). Per-workflow setup lives in each folder's README:

- [`_shared/issue-triage`](_shared/issue-triage) - import first, the inlets call it
- [`slack-linear-triage`](slack-linear-triage)
- [`retro-ingest`](retro-ingest)
- [`occurrence-digest`](occurrence-digest)
