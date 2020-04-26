FROM tubitony/pillow-mozjpeg:7.0.0-3.3.1

WORKDIR /tmp
RUN wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb \
  && dpkg -i erlang-solutions_2.0_all.deb

ENV ELIXIR_VERSION="1.10.2"
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install \
  esl-erlang=1:22.3.2-1 \
  elixir=${ELIXIR_VERSION}-1 \
  # cleanup
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /

ENTRYPOINT ["iex"]
