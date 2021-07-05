FROM node:lts-alpine // lts-alpine 리눅스 기반 node 이미지 사용

ENV NODE_ENV production // 환경변수 production으로 셋팅
ENV NPM_CONFIG_LOGLEVEL warn // 

RUN mkdir /home/node/app/ && chown -R node:node /home/node/app

WORKDIR /home/node/app // app으로 현재 위치 이동

COPY package.json package.json // package.json을 복사
COPY package-lock.json package-lock.json // package-lock.json을 복사

USER node // 사용자 계정을 node로 변경

RUN npm install --production // 프로덕션 모드로 install

// 컨테이너에 .next와 public 폴더 복사
COPY --chown=node:node .next .next // 이걸 build하지 않고 해주는 이유는 .env같은 파일이 있을 경우가 있다면 build는 실패하기 때문.
COPY --chown=node:node public public

EXPOSE 3000 // 해당 컨테이너 내부에서 유효한 포트 지정

CMD npm start // start