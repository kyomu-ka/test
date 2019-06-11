<template>
  <div class="container">
    <!-- 親のpropsを受け取る -->
    <h1 class="text-center pb-2">{{ttl}}</h1>
    <!-- submitイベントを無効に -->
    <form @submit.prevent="">
      <div class="form-group">
        <label for="name">名前</label>
        <input type="text" class="form-control" id="name" v-model="data.name" @change="createMessage()">
      </div>
      <div class="form-group">
        <label for="gakka">所属学科</label>
        <input type="text" class="form-control" id="gakka" v-model="data.gakka" @change="createMessage()">
      </div>
      <div class="form-group">
        <label for="sotugyou">卒業した年</label>
        <input type="text" class="form-control" id="sotugyou" v-model="data.sotugyou" @change="createMessage()">
      </div>
      <div class="form-group">
        <label for="umare">生年月日</label>
        <input type="text" class="form-control" id="umare" v-model="data.umare" @change="createMessage()">
      </div>
      
      <div class="form-group">
        <label for="reason">証明書種類</label>
        <select class="form-control" v-model="data.reason" id="reason" @change="createMessage()">
            <option v-for="(reason, index) in reasons" :key="index">{{reason}}</option>
          </select>
      </div>
      <!-- 証明書種類がその他の時のみ表示 -->
      <div class="form-group" v-show="data.reason=='その他'">
        <input type="text" class="form-control" id="reson_other" @change="createMessage()" placeholder="その他の場合は証明書種類を書いてください。" v-model="data.reason_other">
      </div>
      <div class="form-group">
        <label for="busuu">部数</label>
        <input type="text" class="form-control" id="busuu" v-model="data.busuu" @change="createMessage()">
      </div>

      <div class="form-group">
        <label for="watasi">受け渡し方法</label>
        <select class="form-control" v-model="data.watasi" id="watasi" @change="createMessage()">
            <option v-for="(watasi, index) in watasis" :key="index">{{watasi}}</option>
          </select>
      </div>

      <div class="form-group">
        <label for="yuubin">郵便番号</label>
        <input type="text" class="form-control" id="yuubin" v-model="data.yuubin" @change="createMessage()">
      </div>

      <div class="form-group">
        <label for="juusyo">住所</label>
        <input type="text" class="form-control" id="juusyo" v-model="data.juusyo" @change="createMessage()">
      </div>

      <div class="form-group">
        <label for="denwa">電話番号</label>
        <input type="text" class="form-control" id="denwa" v-model="data.denwa" @change="createMessage()">
      </div>

      <div class="form-group">
        <label for="message">メッセージ</label>
        <!-- createMessageで生成されたデータを挿入 -->
        <textarea class="form-control" id="message" rows="5" v-model="data.message"></textarea>
      </div>
      <div class="form-group pt-2">
        <button class="btn btn-primary btn-lg btn-block" id="send" type="submit">送信</button>
      </div>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'InputForm',
    busuu: 'InputForm',
    yuubin: 'InputForm',
    juusyo: 'InputForm',
    props: {
      ttl: String
    },
    data() {
      return {
        data: {
          name: '',
          gakka: '',
          sotugyou: '',
          umare: '',
          busuu: '',
          reason: '',
          yuubin: '',
          watasi: '',
          juusyo: '',
          reason_other: '',
          denwa: '',
          message: '',
        },
        // 証明書種類
        reasons: ['卒業証明書(和文) 100円', '成績証明書(和文) 200円',  '卒業証明書(和文)＆成績証明書(和文) 300円', '卒業証明書(英文) 500円', '成績証明書(英文) 500円','卒業証明書(英文)&成績証明書(英文) 1,000円','その他'],
        // 郵送方法
        watasis: ['郵送 360円', '窓口渡し'],
        // 種別の中身
        types: ['郵送を希望します', '窓口受取りを希望します'],
        // メッセージのテンプレート
        text2: 'よろしくお願いします。'
      };
    },
    methods: {
      // 時間のオプションを配列で生成
      createTimes: function(init, to) {
        const times = [];
        for (let index = init; index < to; index++) {
          times.push(`${index * 5}分`);
        }
        return times;
      },
      // inputに変更があった場合にメッセージの作成
      createMessage: function() {
        let text, text_reson

        // その他の場合は、reason_otherのデータを使用
        if (this.data.reason == "その他") {
          text_reson = this.data.reason_other
        } else {
          text_reson = this.data.reason
        }

        // メッセージを生成
        text = `以下のとおり証明書を申し込みます。
【所属学科】${this.data.sotugyou}【卒業年】${this.data.sotugyou}
【氏名】${this.data.name}【生年月日】${this.data.umare}
【郵便番号】〒${this.data.yuubin}
【住所】${this.data.juusyo}
【電話番号】${this.data.denwa}
【証明書種類】${text_reson}【部数】${this.data.busuu}
【受け渡し方法】${this.data.watasi}
${this.text2}`;

        // データに渡す
        this.data.message = text;
      },
    }
  };
</script>

<style>

</style>