# EN PROD
docker-compose -p angular-prod -f docker-compose-prod.yaml up --build --force-recreate

# EN DEV
docker-compose -p angular-dev -f docker-compose.yaml up
