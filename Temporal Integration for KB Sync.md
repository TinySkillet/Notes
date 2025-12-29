## Goal Description

Migrate the Knowledge Base synchronization process for both Bedrock and Milvus providers to Temporal workflows. This will ensure reliability, scalability, and better status tracking for long-running ingestion processes.

## Proposed Architecture

### 1. Project Structure

We will add a new `app/temporal` directory to house workflows and activities.

app/temporal/

├── __init__.py

├── worker.py           # Entry point for the Temporal worker

├── workflows/

│   ├── __init__.py

│   ├── bedrock_sync.py # Workflow for Bedrock sync

│   └── milvus_sync.py  # Workflow for Milvus sync

└── activities/

    ├── __init__.py

    ├── bedrock.py      # Activities for Bedrock (start job, check status)

    └── milvus.py       # Activities for Milvus (fetch, chunk, embed, store)

### 2. Bedrock Sync Workflow

**Workflow Name**: `BedrockSyncWorkflow` **Queue**: `kb-sync-queue`

**Logic**:

1. **Activity**: `start_bedrock_ingestion_job(kb_id, data_source_id)`
    - Calls `bedrock_client.start_ingestion_job`.
    - Returns `ingestionJobId`.
2. **Activity**: `check_bedrock_ingestion_status(kb_id, data_source_id, ingestion_job_id)`
    - Polls `bedrock_client.get_ingestion_job` until status is `COMPLETE` or `FAILED`.
    - _Note_: Since Temporal handles retries and long-running processes, we can implement this as a polling loop within the workflow or a recurring activity.

### 3. Milvus Sync Workflow

**Workflow Name**: `MilvusSyncWorkflow` **Queue**: `kb-sync-queue`

**Logic**:

1. **Activity**: `fetch_files_for_ingestion(kb_id, user_id, ds_provider)`
    - Returns list of file metadata/paths.
2. **Activity**: `parse_and_chunk_files(files)`
    - Downloads, parses, and splits text.
    - Returns list of text chunks.
3. **Activity**: `generate_embeddings(chunks)`
    - Calls embedding model service.
    - Returns vectors (dense/sparse).
4. **Activity**: `store_vectors_in_milvus(collection_name, vectors, metadata)`
    - Deletes old collection (if needed) and inserts new vectors.

### 4. API Changes


app/routers/knowledge_base.py

- **Endpoint**: `POST /kb/sync`
- **Change**: Instead of calling `KnowledgeBaseService.start_ingestion` directly, it will:
    1. Connect to Temporal Client.
    2. Start the appropriate workflow (`BedrockSyncWorkflow` or `MilvusSyncWorkflow`) asynchronously.
    3. Return the `workflow_id` and `run_id` immediately with a "started" status.



app/routers/status.py

- **Endpoint**: `POST /kb/sync/status`
- **Change**:
    1. Accept `workflow_id` (and optionally `run_id`) in the request.
    2. Query the Temporal Client for the workflow handle.
    3. Return the workflow status (`RUNNING`, `COMPLETED`, `FAILED`, etc.) and any output/result if available.

## Implementation Steps

### Step 1: Setup Temporal Worker

- Create `app/temporal/worker.py`.
- Initialize Temporal Client.
- Register Workflows and Activities.
- Run this worker separately (e.g., `python -m app.temporal.worker`).

### Step 2: Implement Bedrock Workflow & Activities

- Create `app/temporal/activities/bedrock.py` with 
    
    start_ingestion and `check_status`.
- Create `app/temporal/workflows/bedrock_sync.py` to orchestrate them.

### Step 3: Implement Milvus Workflow & Activities

- Refactor `MilvusKnowledgeBase.start_ingestion` logic into distinct activities in `app/temporal/activities/milvus.py`.
- Create `app/temporal/workflows/milvus_sync.py`.

### Step 4: Update API Endpoints

- Modify 
    
    start_kb_ingestion in 
    
    app/routers/knowledge_base.py to trigger workflows.
- Update 
    
    fetch_injestion_job_status in 
    
    app/routers/status.py to check Temporal status.