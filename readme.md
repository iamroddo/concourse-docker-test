# Start Concourse in local Docker
docker-compose up -d

# Create Concourse target
fly -t concourse-docker-test login --concourse-url http://127.0.0.1:8080 -u admin -p admin

# Create/set pipeline from pipeline.yaml with credentials file (credentials.yaml)
fly -t concourse-docker-test sp -c pipeline.yaml -l credentials.yaml -p docker-build-push

# Unpause pipeline
fly -t concourse-docker-test up -p docker-build-push
fly -t concourse-docker-test trigger-job -j docker-build-push/publish -w

# Clean up and shutdown
docker-compose down
