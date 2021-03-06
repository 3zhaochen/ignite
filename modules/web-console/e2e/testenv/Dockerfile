#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM node:10-alpine

ENV NPM_CONFIG_LOGLEVEL error

# Update package list & install.
RUN apk add --no-cache nginx

# Install global node packages.
RUN npm install -g pm2

# Copy nginx config.
COPY e2e/testenv/nginx/nginx.conf /etc/nginx/nginx.conf
COPY e2e/testenv/nginx/web-console.conf /etc/nginx/web-console.conf

WORKDIR /opt/web-console

# Install node modules for frontend and backend modules.
COPY backend/package*.json backend/
RUN (cd backend && npm install --no-optional --production)

COPY frontend/package*.json frontend/
RUN (cd frontend && npm install --no-optional)

# Copy source.
COPY backend backend
COPY frontend frontend

RUN (cd frontend && npm run build)

EXPOSE 9001

WORKDIR /opt/web-console/backend

CMD nginx && pm2-runtime index.js -n web-console-backend
