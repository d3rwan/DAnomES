FROM kibana:5.0.0-alpha3

MAINTAINER d3rwan

# install plugins
RUN /opt/kibana/bin/kibana-plugin install timelion \
  && chown -R kibana:kibana /opt/kibana


