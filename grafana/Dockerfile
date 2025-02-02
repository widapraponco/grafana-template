# Use official Grafana image
FROM grafana/grafana:11.3.4

# Set environment variables
ENV GF_SECURITY_ADMIN_USER=admin \
    GF_SECURITY_ADMIN_PASSWORD=admin \
    GF_USERS_ALLOW_SIGN_UP=false \
    GF_PLUGINS_ALLOW_LOADING_UNSIGNED_PLUGINS=my-custom-plugin

# Copy the custom plugin into the plugins directory
COPY my-custom-plugin /var/lib/grafana/plugins/my-custom-plugin

# Add provisioning
ADD provisioning /etc/grafana/provisioning
# Add configuration file
ADD grafana.ini /etc/grafana/grafana.ini
# Add dashboard json files
ADD dashboards /etc/grafana/dashboards
# Add assets file
ADD assets /usr/share/grafana/public/img

COPY dashboards/Ngawi/home_dashboard.json /usr/share/grafana/public/dashboards/default.json

## Paths
ENV GF_PATHS_PROVISIONING="/etc/grafana/provisioning"
ENV GF_PATHS_PLUGINS="/var/lib/grafana/plugins"

## Copy Provisioning
COPY --chown=grafana:root provisioning $GF_PATHS_PROVISIONING

USER root

RUN sed -i 's|Main Org.|NgawiKab|g' /usr/share/grafana/bin/grafana-server

RUN sed -i "s|\[\[.NavTree\]\],|nav,|g; \
    s|window.grafanaBootData = {| \
    let nav = [[.NavTree]]; \
    nav[nav.length -1]['subTitle'] = 'DE Kabupaten Ngawi v1.0'; \
    window.grafanaBootData = {|g" \
    /usr/share/grafana/public/views/index.html

RUN sed -i 's|<title>\[\[.AppTitle\]\]</title>|<title>Dashboard Pemkab. Ngawi</title>|g' /usr/share/grafana/public/views/index.html
RUN sed -i 's|<div class="preloader__text">Loading Grafana</div>|<div class="preloader__text">Loading Dashboard</div>|g' /usr/share/grafana/public/views/index.html

## Update Title
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|AppTitle="Grafana"|AppTitle="Dashboard Pemkab. Ngawi"|g' {} \;

## Update Login Title
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|LoginTitle="Welcome to Grafana"|LoginTitle="Selamat Datang di Dashboard Pemkab. Ngawi"|g' {} \;

## Remove Documentation, Support, Community in the Footer
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|\[{target:"_blank",id:"documentation".*grafana_footer"}\]|\[\]|g' {} \;

## Remove Edition in the Footer
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|({target:"_blank",id:"license",.*licenseUrl})|()|g' {} \;

## Remove Version in the Footer
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|({target:"_blank",id:"version",.*CHANGELOG.md":void 0})|()|g' {} \;

## Remove New Version is available in the Footer
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|({target:"_blank",id:"updateVersion",.*grafana_footer"})|()|g' {} \;

## Remove News icon
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|..createElement(....,{className:.,onClick:.,iconOnly:!0,icon:"rss","aria-label":"News"})|null|g' {} \;

## Remove Open Source icon
RUN find /usr/share/grafana/public/build/ -name *.js -exec sed -i 's|.push({target:"_blank",id:"version",text:`${..edition}${.}`,url:..licenseUrl,icon:"external-link-alt"})||g' {} \;

##################################################################
## CLEANING Remove Native Panels
##################################################################

## Alert list
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/alertlist

## Annotations list
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/annolist

## Dashboard list
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/dashlist

## News
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/news

## Table (old)
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/table-old

## Traces
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/traces

## Flamegraph
RUN rm -rf /usr/share/grafana/public/app/plugins/panel/flamegraph

##################################################################

# USER grafana

## Entrypoint
# ENTRYPOINT [ "/bin/bash", "/entrypoint.sh" ]