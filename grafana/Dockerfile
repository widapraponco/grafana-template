# Use the official Grafana Enterprise image as base
FROM grafana/grafana-enterprise:11.3.1

# Set environment variables if needed
ENV GF_SECURITY_ADMIN_USER=admin \
    GF_SECURITY_ADMIN_PASSWORD=admin \
    GF_USERS_ALLOW_SIGN_UP=false \
    GF_INSTALL_PLUGINS="https://github.com/nikos/grafana-percent-trend-panel/releases/download/v1.0.8/nikosc-percenttrend-panel-1.0.8.zip;" \
    GF_PLUGINS_ALLOW_LOADING_UNSIGNED_PLUGINS=wp-dynamic-geojson

# Set Grafana to run in development mode
ENV GF_DEFAULT_APP_MODE=development
# For example, to install plugins:
# ENV GF_INSTALL_PLUGINS="grafana-clock-panel,grafana-simple-json-datasource"

COPY ./plugins/map-panel /var/lib/grafana/plugins/map-panel
# Copy custom configuration file if you have one
COPY provisioning /etc/grafana/provisioning
COPY dashboards/Ngawi /var/lib/grafana/dashboards
ENV GF_DASHBOARDS_DEFAULT_HOME_DASHBOARD_PATH=/var/lib/grafana/dashboards/home_dashboard.json

COPY grafana.ini /etc/grafana/grafana.ini
# Optionally, you can also set the owner of the files
# RUN chown -R grafana:grafana /etc/grafana/provisioning
COPY assets /usr/share/grafana/public/img

# Expose the Grafana port
EXPOSE 3000

# Start Grafana
CMD ["grafana-server", "--homepath=/usr/share/grafana"]