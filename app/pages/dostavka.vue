<script setup lang="ts">
import { site, delivery } from '~/data/site'

useSeoMeta({
  title: `Доставка й оплата — ${site.name}`,
  description:
    'Відправляємо мед Новою поштою та Укрпоштою по всій Україні. Оплата накладеним платежем або на картку. Доставку оплачує замовник за тарифами перевізника.',
  ogTitle: `Доставка й оплата — ${site.name}`,
})
</script>

<template>
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h1>Доставка й оплата</h1>
        <p class="lede">
          Відправляємо по всій Україні. Без передоплати за товар, якщо обираєте накладений платіж.
        </p>
      </div>

      <div class="grid grid--2">
        <div>
          <h2>Чим відправляємо</h2>
          <div class="stack">
            <article v-for="c in delivery.carriers" :key="c.name" class="card">
              <h3>{{ c.name }}</h3>
              <p class="muted" style="font-size: 0.95rem">{{ c.note }}</p>
            </article>
          </div>
        </div>

        <div>
          <h2>Як платити</h2>
          <div class="stack">
            <article v-for="p in delivery.payment" :key="p.name" class="card">
              <h3>{{ p.name }}</h3>
              <p class="muted" style="font-size: 0.95rem">{{ p.note }}</p>
            </article>
          </div>
        </div>
      </div>

      <div class="alert alert--info" style="margin-top: 32px; max-width: 68ch">
        <p><b>Вартість доставки.</b> {{ delivery.whoPays }} Ми не додаємо до неї націнки.</p>
      </div>
    </div>
  </section>

  <section class="section section--tint">
    <div class="wrap prose">
      <h2>Часті питання</h2>

      <h3>Як швидко ви відправляєте?</h3>
      <p>
        Наступного робочого дня після того, як підтвердимо замовлення по телефону. Якщо потрібно
        терміново — скажіть, постараємося відправити того ж дня.
      </p>

      <h3>Чи не розіб’ється банка в дорозі?</h3>
      <p>
        Пакуємо в бульбашкову плівку та щільний картон. За кілька років відправок битих посилок не
        було. Якщо все ж таке станеться — відправимо заміну за свій рахунок.
      </p>

      <h3>Що, як мед прийде зацукрований?</h3>
      <p>
        Це нормальний стан натурального меду, а не псування. Щоб знову став рідким, потримайте
        банку в теплій воді до 40 °C. Не гріть на плиті й не ставте в мікрохвильовку — втратите
        частину корисних властивостей.
      </p>

      <h3>Чи можна забрати самовивозом?</h3>
      <p>
        Так, за адресою {{ site.address }}. Зателефонуйте заздалегідь, щоб ми були на місці й
        підготували замовлення.
      </p>

      <h3>Чи є гуртові ціни?</h3>
      <p>
        Для замовлень від 20 кг обговорюємо окремо — зателефонуйте
        <a :href="`tel:${site.phoneHref}`">{{ site.phone }}</a>.
      </p>

      <h3>Скільки зберігається мед?</h3>
      <p>
        У щільно закритій банці, в темному місці при кімнатній температурі — роками. Головне, щоб
        не потрапляла волога: мед її вбирає й може почати бродити.
      </p>
    </div>
  </section>

  <section class="section">
    <div class="wrap">
      <div class="cta-band">
        <h2>Готові замовити?</h2>
        <p>Порахуйте вартість у калькуляторі й залиште заявку — передзвонимо й усе узгодимо.</p>
        <p style="margin-top: 24px; margin-bottom: 0">
          <NuxtLink to="/produktsiia#rozrahunok" class="btn btn--primary">
            До калькулятора
          </NuxtLink>
        </p>
      </div>
    </div>
  </section>
</template>
