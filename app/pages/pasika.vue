<script setup lang="ts">
import { site, apiaryFacts, rules } from '~/data/site'

useSeoMeta({
  title: `Про пасіку — ${site.name}, с. Солочин`,
  description:
    'Пасіка в с. Солочин Свалявського району на висоті 400 м: близько 60 сімей карпатки. Як ми качаємо мед і чим це відрізняється від поширеної практики.',
  ogTitle: `Про пасіку — ${site.name}`,
})
</script>

<template>
  <!-- ─── Заголовок і паспорт пасіки ──────────────────────────────────── -->
  <section class="section">
    <div class="wrap">
      <div class="apiary__intro">
        <div>
          <h1>Пасіка, а не бренд</h1>
          <p class="lede">
            Одна пасіка в одному селі. Усе, що ми продаємо, зібрано цими бджолами — ми нічого не
            докуповуємо й не перепродаємо.
          </p>
          <p class="muted">
            Солочин лежить у долині, оточеній горами. Тут немає великих сільськогосподарських
            полів — натомість є полонини, вирубки, що заростають малиною, старі липи по схилах і
            акація внизу долини. Тому мед звідси інакший, ніж степовий: складніший і менш солодкий.
          </p>
          <p class="muted">
            Ми тримаємо пасіку як сімейну справу, а не як виробництво. Це означає, що ми знаємо
            кожну сім’ю в обличчя: бачимо, коли вона слабне, коли готова роїтися, а коли її краще
            не чіпати. Через це пасіка росте повільно — ми свідомо не збільшуємо кількість вуликів
            більше, ніж можемо оглянути власноруч за вихідні.
          </p>
          <p class="muted">
            Сезон тут короткий: від акації наприкінці травня до останньої качки в серпні минає
            менш як сто днів. Решту року робота нікуди не дівається — восени готуємо сім’ї до
            зимівлі, взимку лагодимо рамки й топимо вощину, навесні виходжуємо тих, хто перезимував
            слабким. Тому кількість меду щороку різна, і ми не обіцяємо того, чого бджоли не
            принесли: якщо сорт закінчився, ми так і кажемо, а не підмішуємо схожий.
          </p>
        </div>

        <!-- Сухі факти замість загальних слів — оформлені як бирка на банці -->
        <aside class="passport">
          <div class="passport__head">
            <svg width="17" height="19" viewBox="0 0 17 19" fill="none" aria-hidden="true">
              <path
                d="M8.5 1 16 5.25v8.5L8.5 18 1 13.75v-8.5L8.5 1Z"
                stroke="currentColor"
                stroke-width="1.6"
                stroke-linejoin="round"
              />
            </svg>
            Паспорт пасіки
          </div>

          <dl class="passport__list">
            <div v-for="f in apiaryFacts" :key="f.label" class="passport__row">
              <dt>{{ f.label }}</dt>
              <dd>{{ f.value }}</dd>
            </div>
          </dl>

          <p class="passport__note">Дані станом на цей сезон. Приїжджайте — покажемо все на місці.</p>
        </aside>
      </div>
    </div>
  </section>

  <!-- ─── Як зазвичай / як у нас ──────────────────────────────────────── -->
  <section class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Шість рішень, які видно в банці</h2>
        <p>
          Майже все, що нижче, можна було б зробити інакше — швидше й вигідніше. Ліворуч — як
          зазвичай роблять, праворуч — як робимо ми й чого це коштує.
        </p>
      </div>

      <article v-for="(r, i) in rules" :key="r.title" class="versus">
        <h3>
          <span class="versus__num">{{ String(i + 1).padStart(2, '0') }}</span>
          {{ r.title }}
        </h3>

        <div class="versus__pair">
          <div class="versus__side versus__side--common">
            <span class="versus__tag">Як зазвичай</span>
            <p>{{ r.common }}</p>
          </div>
          <div class="versus__side versus__side--ours">
            <span class="versus__tag">Як у нас</span>
            <p>{{ r.text }}</p>
          </div>
        </div>
      </article>
    </div>
  </section>

  <!-- ─── Три кадри з пасіки ──────────────────────────────────────────── -->
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h2>Як це виглядає</h2>
      </div>

      <div class="grid grid--3">
        <figure class="apiary__shot">
          <PhotoSlot
            src="/images/pasika-vulyky.jpg"
            ratio="3 / 4"
            hint="Загальний план: вулики на тлі гір"
            alt="Ряд вуликів на схилі над селом, за ними — лісисті гори"
          />
          <figcaption>Пасіка стоїть на схилі над селом — звідси бджоли йдуть на полонину.</figcaption>
        </figure>
        <figure class="apiary__shot">
          <PhotoSlot
            src="/images/ramka-zapechatana.jpg"
            ratio="3 / 4"
            hint="Рамка із запечатаними сотами в руках"
            alt="Рамка з медом, наполовину запечатана білою восковою кришечкою, з бджолами на сотах"
          />
          <figcaption>Біла воскова кришечка на сотах — сигнал, що мед дозрів і його можна качати.</figcaption>
        </figure>
        <figure class="apiary__shot">
          <PhotoSlot
            src="/images/oglyad-simei.jpg"
            ratio="3 / 4"
            hint="Качка меду або фасування в банки"
            alt="Бджоляр у захисному костюмі виймає рамку з вулика в саду"
          />
          <figcaption>Перед качкою переглядаємо кожну сім'ю — беремо тільки ті рамки, що вже запечатані.</figcaption>
        </figure>
      </div>
    </div>
  </section>

</template>
