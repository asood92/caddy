# caddy-cloudflaredns
# build of caddy based off official image but with cloudflare dns module baked in

Please see the official Caddy Docker Image for deployment instructions.

Few things to note:

    You should add CLOUDFLARE_EMAIL and CLOUDFLARE_API_TOKEN as environment variables to your docker run command. Example:

       docker run -it --name caddy \
         -p 80:80 \
         -p 443:443 \
         -v caddy_data:/data \
         -v caddy_config:/config \
         -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
         -e CLOUDFLARE_EMAIL=me@example.com \
         -e CLOUDFLARE_API_TOKEN=12345 \
         -e ACME_AGREE=true \
         rem3dy/caddy-cloudflaredns

    You can obtain your Cloudflare API token via the Cloudflare Portal. To create a API token with minimal scope, the following steps are needed:

        Log into your dashboard, go to account settings, create API token

        grant the following permissions:
            Zone / Zone / Read
            Zone / DNS / Edit

    You should add the following to your Caddyfile as the tls directive. Note that I haven't figured out why the using {env.CLOUDFLARE_EMAIL} for the email address didn't work, so for now I've got it manually declared:

    tls me@example.com { 
      dns cloudflare {env.CLOUDFLARE_API_TOKEN}
    }

