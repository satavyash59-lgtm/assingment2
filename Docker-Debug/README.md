#QUESTION 1 

3 ERRORS 

1ST.  USER appuser
      RUN useradd -m appuser
      
      here we use the user before creating it
      
      
      solution:
        
        use this "RUN adduser -D appuser"
        

2nd.   HEALTHCHECK --interval=30s --timeout=3s \
       CMD curl -f http://localhost/ || exit 1
       
       Insead of curl we have to use wget as alpine has preinstalled it
       
       solution:
         HEALTHCHECK --interval=10s --timeout=3s --retries=3 \
         CMD wget -q --spider http://127.0.0.1/ || exit 1
         
         
3rd.    WORKDIR /app
	COPY . .
	EXPOSE 8080

        required to give the complete path 
        
        solution:
          
               COPY index.html /usr/share/nginx/html/index.html
		EXPOSE 80
		CMD ["nginx", "-g", "daemon off;"]
		
		
#Question 2
		
		#Question 2

ERRORS

1ST.  depends_on:
        - database
      
      service name mismatch: the database service is declared as "db:", not "database"
      
      solution:
        
        depends_on:
          - db


2nd.  ports:
        -8080:3000
      environment:
        -APP_ENV=production

      yaml syntax error: missing space after hyphen "-" for list elements, and ports should be quoted
      
      solution:
        
        ports:
          - "8085:3000"
        environment:
          - APP_ENV=production


3rd.  volumes:
        - dbdata:/var/lib/postgresql/data

      missing top-level named volume declaration at the root of the compose file
      
      solution:
        
        add this at the end of the file:
        
        volumes:
          dbdata:


4th.  app environment missing database configuration
      
      the app is meant to connect to the database but lacks connection details to reach the "db" service
      
      solution:
        
        add database environment variables under app service:
        
        environment:
          - APP_ENV=production
          - DB_HOST=db
          - DB_PORT=5432
          - DB_PASSWORD=secret
          
          
          
          
QUESTION 3
 
 SOLUTION:
 
#.env:
		
		POSTGRES_DB=production_db
		POSTGRES_USER=appuser
		POSTGRES_PASSWORD=supersecretpassword123
		WEB_PORT=8085
		
#dockerfile:
		

		FROM nginxinc/nginx-unprivileged:alpine

		WORKDIR /usr/share/nginx/html
		COPY index.html .


		HEALTHCHECK --interval=10s --timeout=3s --retries=3 \
		  CMD wget -q --spider http://127.0.0.1:8080/ || exit 1

		EXPOSE 8080

		CMD ["nginx", "-g", "daemon off;"]
		
		
#compose.yaml
		
		services:
		  web:
		    build: .
		    restart: unless-stopped
		    ports:
		      - "${WEB_PORT:-8085}:8080"
		    deploy:
		      resources:
			limits:
			  memory: 256M
		    depends_on:
		      db:
			condition: service_healthy

		  db:
		    image: postgres:alpine
		    restart: unless-stopped
		    environment:
		      POSTGRES_DB: ${POSTGRES_DB}
		      POSTGRES_USER: ${POSTGRES_USER}
		      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
		    volumes:
		      - pgdata:/var/lib/postgresql
		    healthcheck:
		      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
		      interval: 5s
		      timeout: 5s
		      retries: 5
		      start_period: 20s

		volumes:
		  pgdata:
