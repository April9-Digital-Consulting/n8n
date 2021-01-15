# 1. Create an image to build n8n
FROM node:12.16-alpine as builder

# Update everything and install needed dependencies
USER root

# Install all needed dependencies
RUN apk --update add --virtual build-dependencies python build-base ca-certificates && \
	npm_config_user=root npm install -g lerna

WORKDIR /data

COPY lerna.json .
COPY package.json .
COPY packages/cli/ ./packages/cli/
COPY packages/core/ ./packages/core/
COPY packages/editor-ui/ ./packages/editor-ui/
COPY packages/nodes-base/ ./packages/nodes-base/
COPY packages/workflow/ ./packages/workflow/
RUN rm -rf node_modules packages/*/node_modules packages/*/dist

ENV BASE_PATH="/workflow_engine/" VUE_APP_URL_BASE_API="/workflow_engine/"
RUN npm install --production --loglevel notice
RUN lerna bootstrap --hoist -- --production
RUN npm run build


# 2. Start with a new clean image with just the code that is needed to run n8n
FROM node:12.16-alpine
ENV N8N_PATH="/workflow_engine/"
ENV NODES_EXCLUDE="["n8n-nodes-base.executeCommand","n8n-nodes-base.writeBinaryFile","n8n-nodes-base.moveBinaryData","n8n-nodes-base.openWeatherMap","n8n-nodes-base.readBinaryFile","n8n-nodes-base.readBinaryFiles","n8n-nodes-base.renameKeys","n8n-nodes-base.rssFeedRead","n8n-nodes-base.splitInBatches","n8n-nodes-base.sseTrigger","n8n-nodes-base.activeCampaign","n8n-nodes-base.activeCampaignTrigger","n8n-nodes-base.acuitySchedulingTrigger","n8n-nodes-base.affinity","n8n-nodes-base.affinityTrigger","n8n-nodes-base.agileCrm","n8n-nodes-base.airtable","n8n-nodes-base.airtableTrigger","n8n-nodes-base.amqp","n8n-nodes-base.amqpTrigger","n8n-nodes-base.asana","n8n-nodes-base.asanaTrigger","n8n-nodes-base.automizy","n8n-nodes-base.awsRekognition","n8n-nodes-base.bannerbear","n8n-nodes-base.bitbucketTrigger","n8n-nodes-base.bitly","n8n-nodes-base.box","n8n-nodes-base.boxTrigger","n8n-nodes-base.Brandfetch","n8n-nodes-base.calendlyTrigger","n8n-nodes-base.chargebee","n8n-nodes-base.chargebeeTrigger","n8n-nodes-base.circleCi","n8n-nodes-base.clearbit","n8n-nodes-base.clickUp","n8n-nodes-base.clickUpTrigger","n8n-nodes-base.clockify","n8n-nodes-base.clockifyTrigger","n8n-nodes-base.cockpit","n8n-nodes-base.coda","n8n-nodes-base.coinGecko","n8n-nodes-base.contentful","n8n-nodes-base.convertKit","n8n-nodes-base.convertKitTrigger","n8n-nodes-base.copperTrigger","n8n-nodes-base.cortex","n8n-nodes-base.crateDb","n8n-nodes-base.customerIo","n8n-nodes-base.customerIoTrigger","n8n-nodes-base.discord","n8n-nodes-base.disqus","n8n-nodes-base.drift","n8n-nodes-base.dropbox","n8n-nodes-base.egoi","n8n-nodes-base.eventbriteTrigger","n8n-nodes-base.filemaker","n8n-nodes-base.flow","n8n-nodes-base.flowTrigger","n8n-nodes-base.freshdesk","n8n-nodes-base.getResponse","n8n-nodes-base.ghost","n8n-nodes-base.github","n8n-nodes-base.githubTrigger","n8n-nodes-base.gitlab","n8n-nodes-base.gitlabTrigger","n8n-nodes-base.googleBooks","n8n-nodes-base.googleCalendar","n8n-nodes-base.googleCloudNaturalLanguage","n8n-nodes-base.googleContacts","n8n-nodes-base.googleDrive","n8n-nodes-base.googleFirebaseRealtimeDatabase","n8n-nodes-base.gSuiteAdmin","n8n-nodes-base.googleTasks","n8n-nodes-base.googleTranslate","n8n-nodes-base.youTube","n8n-nodes-base.gotify","n8n-nodes-base.gumroadTrigger","n8n-nodes-base.hackerNews","n8n-nodes-base.harvest","n8n-nodes-base.helpScout","n8n-nodes-base.helpScoutTrigger","n8n-nodes-base.hubspot","n8n-nodes-base.hubspotTrigger","n8n-nodes-base.humanticAi","n8n-nodes-base.hunter","n8n-nodes-base.intercom","n8n-nodes-base.invoiceNinja","n8n-nodes-base.invoiceNinjaTrigger","n8n-nodes-base.iterable","n8n-nodes-base.jira","n8n-nodes-base.jiraTrigger","n8n-nodes-base.jotFormTrigger","n8n-nodes-base.kafka","n8n-nodes-base.kafkaTrigger","n8n-nodes-base.keap","n8n-nodes-base.keapTrigger","n8n-nodes-base.line","n8n-nodes-base.lingvaNex","n8n-nodes-base.linkedIn","n8n-nodes-base.mailchimp","n8n-nodes-base.mailchimpTrigger","n8n-nodes-base.mailerLite","n8n-nodes-base.mailerLiteTrigger","n8n-nodes-base.mailgun","n8n-nodes-base.mailjet","n8n-nodes-base.mailjetTrigger","n8n-nodes-base.mandrill","n8n-nodes-base.matrix","n8n-nodes-base.mattermost","n8n-nodes-base.mautic","n8n-nodes-base.mauticTrigger","n8n-nodes-base.medium","n8n-nodes-base.messageBird","n8n-nodes-base.microsoftOneDrive","n8n-nodes-base.mindee","n8n-nodes-base.mocean","n8n-nodes-base.mondayCom","n8n-nodes-base.mqttTrigger","n8n-nodes-base.msg91","n8n-nodes-base.nasa","n8n-nodes-base.nextCloud","n8n-nodes-base.openThesaurus","n8n-nodes-base.orbit","n8n-nodes-base.paddle","n8n-nodes-base.pagerDuty","n8n-nodes-base.payPalTrigger","n8n-nodes-base.phantombuster","n8n-nodes-base.philipsHue","n8n-nodes-base.pipedrive","n8n-nodes-base.pipedriveTrigger","n8n-nodes-base.postmarkTrigger","n8n-nodes-base.profitWell","n8n-nodes-base.pushbullet","n8n-nodes-base.pushcut","n8n-nodes-base.pushcutTrigger","n8n-nodes-base.pushover","n8n-nodes-base.questDb","n8n-nodes-base.quickbase","n8n-nodes-base.rabbitmq","n8n-nodes-base.rabbitmqTrigger","n8n-nodes-base.rocketchat","n8n-nodes-base.rundeck","n8n-nodes-base.salesforce","n8n-nodes-base.salesmate","n8n-nodes-base.segment","n8n-nodes-base.sendy","n8n-nodes-base.sentryIo","n8n-nodes-base.shopify","n8n-nodes-base.shopifyTrigger","n8n-nodes-base.signl4","n8n-nodes-base.slack","n8n-nodes-base.sms77","n8n-nodes-base.snowflake","n8n-nodes-base.spontit","n8n-nodes-base.spotify","n8n-nodes-base.storyblok","n8n-nodes-base.strapi","n8n-nodes-base.strava","n8n-nodes-base.stravaTrigger","n8n-nodes-base.stripeTrigger","n8n-nodes-base.surveyMonkeyTrigger","n8n-nodes-base.taiga","n8n-nodes-base.taigaTrigger","n8n-nodes-base.telegram","n8n-nodes-base.telegramTrigger","n8n-nodes-base.theHive","n8n-nodes-base.theHiveTrigger","n8n-nodes-base.todoist","n8n-nodes-base.togglTrigger","n8n-nodes-base.travisCi","n8n-nodes-base.trello","n8n-nodes-base.trelloTrigger","n8n-nodes-base.twake","n8n-nodes-base.twist","n8n-nodes-base.twitter","n8n-nodes-base.typeformTrigger","n8n-nodes-base.unleashedSoftware","n8n-nodes-base.uplead","n8n-nodes-base.uproc","n8n-nodes-base.vero","n8n-nodes-base.vonage","n8n-nodes-base.webflowTrigger","n8n-nodes-base.wekan","n8n-nodes-base.wooCommerce","n8n-nodes-base.wooCommerceTrigger","n8n-nodes-base.wordpress","n8n-nodes-base.wufooTrigger","n8n-nodes-base.yourls","n8n-nodes-base.zohoCrm","n8n-nodes-base.zulip"]"
USER root

RUN apk add --update graphicsmagick tzdata tini su-exec

WORKDIR /data

# Install all needed dependencies
RUN npm_config_user=root npm install -g full-icu

# Install fonts
RUN apk --no-cache add --virtual fonts msttcorefonts-installer fontconfig && \
	update-ms-fonts && \
	fc-cache -f && \
	apk del fonts && \
	find  /usr/share/fonts/truetype/msttcorefonts/ -type l -exec unlink {} \;

ENV NODE_ICU_DATA /usr/local/lib/node_modules/full-icu

COPY --from=builder /data ./

COPY docker/images/n8n-custom/docker-entrypoint.sh /docker-entrypoint.sh
ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]

EXPOSE 5678/tcp
