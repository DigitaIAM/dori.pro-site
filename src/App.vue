<script setup>
import { ref, watch, computed, nextTick, onMounted, onBeforeUnmount } from 'vue'

const navLinks = [
  { label: 'О системе', href: '#products' },
  { label: 'Преимущества', href: '#features' },
  { label: 'Проблемы и решения', href: '#problems' },
]

const industries = ['Аптечных сетей', 'Фармацевтики', 'Ритейла', 'Дистрибуции']

const mobileMenuOpen = ref(false)
const closeMobileMenu = () => {
  mobileMenuOpen.value = false
}

// Управление активным проектом в модалке
const activeProject = ref(null)

const openProject = (project) => {
  activeProject.value = project
  mobileMenuOpen.value = false
}

const closeProject = () => {
  activeProject.value = null
}

watch(activeProject, (project) => {
  document.body.style.overflow = project ? 'hidden' : ''
})

const projectLabels = {
  dori: 'Фармацевтика & Ритейл',
}
const projectLabel = computed(() => projectLabels[activeProject.value] || '')

// Аккордеоны: на десктопе раскрыты, на мобильных схлопнуты
const accordionQuery = window.matchMedia('(min-width: 640px)')
const syncAccordions = () => {
  const open = accordionQuery.matches
  document.querySelectorAll('details[data-accordion]').forEach((el) => {
    el.open = open
  })
}

onMounted(syncAccordions)
accordionQuery.addEventListener('change', syncAccordions)
onBeforeUnmount(() => accordionQuery.removeEventListener('change', syncAccordions))
watch(activeProject, () => nextTick(syncAccordions))

const features = [
  {
    title: 'Облачная доступность 24/7',
    text: 'Работа прямо в браузере с любого устройства без установки и серверов. Филиалы и сотрудники видят актуальные данные одновременно в реальном времени.',
    accent: 'brand',
    icon: 'M7.5 18a4.5 4.5 0 01-.5-8.97A5.5 5.5 0 0117.8 10.5 3.75 3.75 0 0117 18H7.5z',
  },
  {
    title: 'Умное автораспределение',
    text: 'Алгоритмы сами рассчитывают потребность точек по истории продаж и распределяют товар по приоритетам («горит», «нужно», «можно») с учётом сроков годности (FEFO).',
    accent: 'sage',
    icon: 'M12 6a4 4 0 1 1 0 8a4 4 0 1 1 0-8zM10 14c0 1 .3 1.8.8 2.2h2.4c.5-.4.8-1.2.8-2.2M10 17h4M10.5 19.5h3M12 1.5v2M5.4 3.9l1.5 1.5M18.6 3.9l-1.5 1.5M4 10h1.5M18.5 10H20',
  },
  {
    title: 'Автоматизация расчётов',
    text: 'Забудьте про ошибки ручного счёта в Excel. Взаиморасчёты с поставщиками, сверки, бонусы и финансовые итоги формируются автоматически.',
    accent: 'brand',
    icon: 'M4 19V5M4 19h16M8 15l3-4 3 2 4-6',
  },
  {
    title: 'Устойчивость к сбоям связи',
    text: 'Кассовые терминалы продолжают работать даже при временном обрыве интернета. При восстановлении связи все данные автоматически синхронизируются и выравниваются.',
    accent: 'sage',
    icon: 'M12 3l7 3v5.5c0 4.4-3 7.5-7 9.5-4-2-7-5.1-7-9.5V6l7-3z',
  },
  {
    title: 'Минимум списаний и потерь',
    text: 'Строгий контроль сроков годности, отслеживание каждой партии от приемки до продажи и предотвращение образования «мертвых» запасов на складах.',
    accent: 'brand',
    icon: 'M22 17 13.5 8.5 8.5 13.5 2 7M16 17h6v-6',
  },
  {
    title: 'Прозрачная отчетность и KPI',
    text: 'Руководство видит актуальные дашборды, показатели эффективности сотрудников, финансовые результаты с возможностью экспорта в Excel/PDF в один клик.',
    accent: 'sage',
    icon: 'M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7zM14 2v4a2 2 0 0 0 2 2h4M10 9H8M16 13H8M16 17H8',
  },
]

const problems = [
  {
    title: 'Проблема на складе',
    quote: 'Склады стоят, сотрудники завалены посылками, никто не понимает, где что находится. Возникают неконтролируемые списания, пересортица и правки документов задним числом…',
    solution: 'Организуем движение товаров по ячейкам с точностью до единицы. Перекрестные проверки и жесткое логирование исключают ошибки, путаницу и несанкционированные правки.',
  },
  {
    title: 'Инвентаризация без простоев',
    quote: 'Нужно делать инвентаризацию, но останавливать работу склада или точек никак нельзя...',
    solution: 'Система позволяет проводить непрерывную инвентаризацию прямо на работающем складе, гарантируя абсолютную точность результатов и исключая пересортицу.',
  },
  {
    title: 'Распределение и просрочка',
    quote: 'В одних точках товар пылится, в других его ждут месяцы. Никто не знает точно объемы просрочки и дефицита...',
    solution: 'Алгоритм с точностью до пачки знает потребности точек, автоматически распределяет товар по приоритетам, строго контролируя партии и сроки годности (FEFO) для минимизации убытков.',
  },
  {
    title: 'Маркировка и DataMatrix',
    quote: 'Сквозная маркировка и DataMatrix введены по бумагам, но складская программа работает только с обычными штрихкодами. Умных сканеров нет из-за цены, а товары без маркировки приходится проводить вручную и верить сотрудникам на слово.',
    solution: 'Работаем со всеми видами маркировок: сборка и упаковка проверяют друг друга, немаркированный товар контролируется протоколами и организацией хранения. Подойдёт любой телефон с камерой.',
  },
  {
    title: 'Просрочка и лицензия',
    quote: 'Государство закрутило гайки, за продажу просрочки светит уголовка и лишение лицензии, операторам приходится вручную проверять каждую пачку. Но они ленятся и ошибаются, а отвечаю я.',
    solution: 'Мы строго контролируем каждую партию, продажа просроченного маркированного товара физически невозможна.',
  },
  {
    title: 'Хаос в прайсах поставщиков',
    quote: 'У каждого поставщика свое описание, единых идентификаторов нет. Сотрудники вручную сопоставляют тысячи строк...',
    solution: 'Интеллектуальная система семантического сопоставления распознает индустриальные сокращения и идиомы. Большинство позиций связываются автоматически, а решения по спорным случаям система запоминает навсегда.',
  },
  {
    title: 'Рассинхрон департаментов',
    quote: 'Документы из логистики и бухгалтерии не бьются между собой, полно ошибок, суммы и количества расходятся...',
    solution: 'Интеллектуальный кросс-анализ данных и протоколов находит истину, автоматически выявляет неточности на ранних стадиях и исключает человеческие ошибки операторов.',
  },
  {
    title: 'Неквалифицированный персонал',
    quote: 'Текучка кадров, невнимательность, сотрудники игнорируют правила и ошибаются, обучение занимает месяцы...',
    solution: 'Система не требует долгого обучения — она сама контролирует действия сотрудника. Сделать что-то неправильно физически не получится, а любые действия прозрачно журналируются.',
  },
  {
    title: 'Задержки в аналитике',
    quote: 'Построение отчетов занимает часы, менеджмент живет в прошлом и не знает реального положения дел. Сторно и операционные ошибки — постоянная головная боль...',
    solution: 'Расчет остатков и ключевых показателей происходит в реальном времени. Сторно и правки прошлых периодов обрабатываются автоматически и мгновенно отражаются в отчетах. Руководство может моделировать сценарии «а что если...» и быстро находить причины изменений.',
  },
  {
    title: 'Совместимость с имеющимся софтом',
    quote: 'Вложены миллионы в текущие ERP, CRM, WMS, TMS, но они плохо дружат между собой. Синхронизация сбоит, а сотрудники ведут документы в нескольких системах одновременно...',
    solution: 'Мы не вынуждаем отказываться от существующего софта. Система строит надежные мосты и интеграции к имеющимся решениям, объединяя данные и отдавая их бизнесу в удобном виде.',
  },
]

// Форма заявки
const form = ref({ name: '', phone: '', company: '', message: '' })
const isSubmitted = ref(false)
const isSubmitting = ref(false)
const submitError = ref(false)

const FORMSPREE_ENDPOINT = 'https://formspree.io/f/xgaonbqz'

const submitForm = async () => {
  if (!form.value.name || !form.value.phone) return
  isSubmitting.value = true
  submitError.value = false

  const data = new FormData()
  data.append('name', form.value.name)
  data.append('phone', form.value.phone)
  data.append('company', form.value.company)
  data.append('message', form.value.message)

  try {
    const res = await fetch(FORMSPREE_ENDPOINT, {
      method: 'POST',
      body: data,
      headers: { Accept: 'application/json' },
    })
    if (res.ok) {
      isSubmitted.value = true
      form.value = { name: '', phone: '', company: '', message: '' }
    } else {
      submitError.value = true
    }
  } catch {
    submitError.value = true
  } finally {
    isSubmitting.value = false
  }
}
</script>

<template>
  <div class="min-h-screen bg-canvas font-sans text-ink antialiased">

    <!-- Навигация -->
    <header class="sticky top-0 z-40 border-b border-line/80 bg-canvas/85 backdrop-blur-xl">
      <div class="site-container flex h-20 items-center justify-between">
        <a href="#" class="flex items-center gap-3" @click="closeMobileMenu">
          <span class="flex h-9 w-9 items-center justify-center rounded-xl bg-ink text-sm font-semibold text-white">D</span>
          <span class="text-lg font-semibold tracking-tight text-ink">Dori<span class="text-brand">.pro</span></span>
        </a>

        <nav class="hidden items-center gap-8 lg:flex">
          <a
            v-for="link in navLinks"
            :key="link.href"
            :href="link.href"
            class="text-sm text-muted transition-colors hover:text-ink"
          >
            {{ link.label }}
          </a>
        </nav>

        <div class="flex items-center gap-3">
          <a
            href="#contact"
            class="hidden rounded-full bg-ink px-5 py-2.5 text-sm font-medium text-white transition-colors hover:bg-brand sm:inline-flex"
          >
            Обсудить проект
          </a>
          <button
            type="button"
            class="flex h-10 w-10 items-center justify-center rounded-full border border-line bg-white text-ink transition hover:border-line-strong lg:hidden"
            :aria-expanded="mobileMenuOpen"
            :aria-label="mobileMenuOpen ? 'Закрыть меню' : 'Открыть меню'"
            @click="mobileMenuOpen = !mobileMenuOpen"
          >
            <svg v-if="!mobileMenuOpen" class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round">
              <path d="M4 7h16M4 12h16M4 17h16" />
            </svg>
            <svg v-else class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round">
              <path d="M6 6l12 12M18 6L6 18" />
            </svg>
          </button>
        </div>
      </div>

      <div v-if="mobileMenuOpen" class="border-t border-line bg-canvas lg:hidden">
        <nav class="site-container flex flex-col py-4">
          <a
            v-for="link in navLinks"
            :key="link.href"
            :href="link.href"
            class="border-b border-line/70 py-3.5 text-sm text-muted transition-colors hover:text-ink"
            @click="closeMobileMenu"
          >
            {{ link.label }}
          </a>
          <a
            href="#contact"
            class="mt-4 inline-flex items-center justify-center rounded-full bg-ink px-5 py-3.5 text-sm font-medium text-white transition-colors hover:bg-brand"
            @click="closeMobileMenu"
          >
            Обсудить проект
          </a>
        </nav>
      </div>
    </header>

    <!-- Hero -->
    <section class="relative overflow-hidden border-b border-line bg-canvas">
      <div class="pointer-events-none absolute inset-x-0 top-0 h-[520px] bg-[radial-gradient(ellipse_at_top_right,rgba(37,99,235,0.07),transparent_55%)]"></div>

      <div class="site-container relative grid items-center gap-14 py-20 lg:grid-cols-[0.95fr_1.05fr] lg:py-28">
        <div>
          <p class="inline-flex items-center gap-2 rounded-full border border-line bg-white/70 px-3.5 py-1.5 text-[11px] font-medium uppercase tracking-[0.16em] text-muted">
            <span class="h-1.5 w-1.5 rounded-full bg-brand"></span>
            Управление аптечной сетью
          </p>

          <h1 class="mt-7 max-w-2xl text-[3.1rem] font-medium leading-[0.98] tracking-[-0.045em] text-ink sm:text-6xl lg:text-[4.6rem]">
            Единое цифровое ядро <span class="text-brand">для аптечной сети</span>
          </h1>

          <p class="mt-7 max-w-xl text-base leading-relaxed text-muted sm:text-lg">
            Dori.pro объединяет кассу, склад, закупки, логистику и руководство в одном цифровом контуре: автоматическое распределение товара по FEFO, контроль сроков годности, точные остатки и отчёты в реальном времени.
          </p>

          <div class="mt-9 flex flex-wrap items-center gap-4">
            <a href="#products" class="inline-flex items-center gap-2 rounded-full bg-ink px-6 py-3.5 text-sm font-medium text-white transition-colors hover:bg-brand">
              Изучить решения
              <svg class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
                <path d="M5 12h14M13 6l6 6-6 6" />
              </svg>
            </a>
            <a href="#contact" class="inline-flex items-center gap-2 px-2 py-3 text-sm font-medium text-ink underline-offset-4 transition hover:underline">
              Обсудить проект
            </a>
          </div>

          <div class="mt-12 grid max-w-md grid-cols-3 gap-4 border-t border-line pt-6">
            <div>
              <p class="text-xl font-medium tracking-tight">24/7</p>
              <p class="mt-1 text-xs text-muted">в облаке</p>
            </div>
            <div>
              <p class="text-xl font-medium tracking-tight">FEFO</p>
              <p class="mt-1 text-xs text-muted">контроль сроков</p>
            </div>
            <div>
              <p class="text-xl font-medium tracking-tight">Real-time</p>
              <p class="mt-1 text-xs text-muted">остатки и отчёты</p>
            </div>
          </div>
        </div>

        <!-- Мокап дашборда -->
        <div class="relative">
          <div class="relative overflow-hidden rounded-[24px] border border-line bg-white shadow-[0_28px_90px_-38px_rgba(13,12,12,0.3)]">
            <div class="flex items-center justify-between border-b border-line px-4 py-3 sm:px-5">
              <div class="flex items-center gap-2.5">
                <span class="flex h-6 w-6 items-center justify-center rounded-lg bg-ink text-[10px] font-semibold text-white">D</span>
                <span class="text-xs font-medium text-ink">Dori.pro · Обзор сети</span>
              </div>
              <span class="inline-flex items-center gap-1.5 rounded-full border border-line px-2.5 py-1 text-[10px] text-muted">
                <span class="h-1.5 w-1.5 rounded-full bg-sage-soft"></span>
                Онлайн
              </span>
            </div>

            <div class="grid grid-cols-[64px_minmax(0,1fr)] sm:grid-cols-[92px_minmax(0,1fr)]">
              <aside class="border-r border-line bg-canvas/70 p-2.5 sm:p-3">
                <div class="flex h-8 items-center justify-center rounded-lg bg-white text-[10px] font-semibold text-ink sm:justify-start sm:px-2.5">DI</div>
                <div class="mt-4 space-y-1.5">
                  <div class="flex items-center gap-2 rounded-lg bg-white px-2 py-2 text-[9px] font-medium text-ink">
                    <span class="h-1.5 w-1.5 rounded-full bg-brand"></span>
                    <span class="hidden sm:inline">Сводка</span>
                  </div>
                  <div class="flex items-center gap-2 px-2 py-2 text-[9px] text-muted">
                    <span class="h-1.5 w-1.5 rounded-full bg-line-strong"></span>
                    <span class="hidden sm:inline">Заказы</span>
                  </div>
                  <div class="flex items-center gap-2 px-2 py-2 text-[9px] text-muted">
                    <span class="h-1.5 w-1.5 rounded-full bg-line-strong"></span>
                    <span class="hidden sm:inline">Склад</span>
                  </div>
                  <div class="flex items-center gap-2 px-2 py-2 text-[9px] text-muted">
                    <span class="h-1.5 w-1.5 rounded-full bg-line-strong"></span>
                    <span class="hidden sm:inline">Отчёты</span>
                  </div>
                </div>
              </aside>

              <div class="min-w-0 p-3.5 sm:p-5">
                <div class="flex items-start justify-between gap-3">
                  <div>
                    <p class="text-[9px] uppercase tracking-[0.14em] text-muted">Сводка</p>
                    <p class="mt-0.5 text-sm font-medium tracking-tight">Аптечная сеть</p>
                  </div>
                  <span class="text-[9px] text-muted">Сегодня</span>
                </div>

                <div class="mt-4 grid grid-cols-3 gap-2">
                  <div class="rounded-lg border border-line p-2.5 sm:p-3">
                    <p class="text-[8px] uppercase tracking-wide text-muted">Продажи</p>
                    <p class="mt-1 text-sm font-medium tracking-tight sm:text-base">2.4M</p>
                    <p class="text-[8px] text-sage">+12.8%</p>
                  </div>
                  <div class="rounded-lg border border-line p-2.5 sm:p-3">
                    <p class="text-[8px] uppercase tracking-wide text-muted">Заказы</p>
                    <p class="mt-1 text-sm font-medium tracking-tight sm:text-base">184</p>
                    <p class="text-[8px] text-brand">+6.2%</p>
                  </div>
                  <div class="rounded-lg border border-line p-2.5 sm:p-3">
                    <p class="text-[8px] uppercase tracking-wide text-muted">Остатки</p>
                    <p class="mt-1 text-sm font-medium tracking-tight sm:text-base">98%</p>
                    <p class="text-[8px] text-sage">в норме</p>
                  </div>
                </div>

                <div class="mt-3 rounded-xl border border-line p-3">
                  <div class="flex items-center justify-between">
                    <p class="text-[10px] font-medium">Динамика продаж</p>
                    <span class="text-[9px] text-muted">7 дней</span>
                  </div>
                  <svg viewBox="0 0 300 76" class="mt-2 h-16 w-full" fill="none" aria-hidden="true">
                    <defs>
                      <linearGradient id="heroChartFill" x1="0" y1="0" x2="0" y2="1">
                        <stop offset="0%" stop-color="#2563eb" stop-opacity="0.16" />
                        <stop offset="100%" stop-color="#2563eb" stop-opacity="0" />
                      </linearGradient>
                    </defs>
                    <path d="M0 18h300M0 38h300M0 58h300" stroke="#e4e4e0" stroke-width="1" />
                    <path d="M0 62c24 0 34-12 52-14s27 9 44 4 25-17 43-20 24 12 43 9 26-15 44-22 25 4 38 2 20-8 36-14v69H0z" fill="url(#heroChartFill)" />
                    <path d="M0 62c24 0 34-12 52-14s27 9 44 4 25-17 43-20 24 12 43 9 26-15 44-22 25 4 38 2 20-8 36-14" stroke="#2563eb" stroke-width="2.5" stroke-linecap="round" />
                    <circle cx="252" cy="29" r="3.5" fill="#fff" stroke="#2563eb" stroke-width="2" />
                  </svg>
                </div>

                <div class="mt-3 space-y-1.5">
                  <div class="flex items-center justify-between rounded-lg border border-line px-2.5 py-2">
                    <div class="flex items-center gap-2">
                      <span class="h-1.5 w-1.5 rounded-full bg-sage-soft"></span>
                      <span class="text-[9px] text-muted">Автораспределение · 14 аптек</span>
                    </div>
                    <span class="text-[8px] text-sage">готово</span>
                  </div>
                  <div class="flex items-center justify-between rounded-lg border border-line px-2.5 py-2">
                    <div class="flex items-center gap-2">
                      <span class="h-1.5 w-1.5 rounded-full bg-brand"></span>
                      <span class="text-[9px] text-muted">FEFO · контроль сроков</span>
                    </div>
                    <span class="text-[8px] text-brand">активно</span>
                  </div>
                  <div class="hidden items-center justify-between rounded-lg border border-line px-2.5 py-2 sm:flex">
                    <div class="flex items-center gap-2">
                      <span class="h-1.5 w-1.5 rounded-full bg-line-strong"></span>
                      <span class="text-[9px] text-muted">Инвентаризация · склад №2</span>
                    </div>
                    <span class="text-[8px] text-muted">в процессе</span>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="absolute -bottom-5 -left-3 hidden items-center gap-3 rounded-xl border border-line bg-white px-4 py-3 shadow-[0_16px_40px_-24px_rgba(13,12,12,0.35)] sm:flex">
            <span class="flex h-8 w-8 items-center justify-center rounded-full bg-sage/10 text-sage">
              <svg class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
                <path d="M5 12.5l4.5 4.5L19 7" />
              </svg>
            </span>
            <div>
              <p class="text-[9px] uppercase tracking-[0.14em] text-muted">Остатки</p>
              <p class="text-xs font-medium">В норме во всех точках</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Отрасли -->
      <div class="site-container relative border-t border-line py-10">
        <div class="flex flex-col gap-6 lg:flex-row lg:items-center lg:justify-between">
          <p class="text-[11px] font-medium uppercase tracking-[0.18em] text-muted">Создаём системы для</p>
          <div class="flex flex-wrap items-center gap-x-3 gap-y-2 lg:w-[68%] lg:justify-end">
            <template v-for="(industry, index) in industries" :key="industry">
              <span class="text-sm font-medium text-ink">{{ industry }}</span>
              <span
                v-if="index < industries.length - 1"
                class="h-1 w-1 rounded-full bg-line-strong"
                aria-hidden="true"
              ></span>
            </template>
          </div>
        </div>
      </div>
    </section>

    <!-- Продукт -->
    <section id="products" class="border-b border-line bg-white py-24 lg:py-32">
      <div class="site-container">
        <div class="grid gap-8 lg:grid-cols-2 lg:items-end">
          <div>
            <p class="text-[11px] font-semibold uppercase tracking-[0.18em] text-brand">Продукт</p>
            <h2 class="mt-4 max-w-xl text-3xl font-medium leading-[1.05] tracking-[-0.035em] text-ink sm:text-4xl lg:text-5xl">Dori.pro
            </h2>
          </div>
          <p class="max-w-xl text-sm leading-relaxed text-muted lg:justify-self-end">
            Единая система для управления аптечной сетью: от кассы и склада до закупок и аналитики — без ручного труда, таблиц Excel и хаоса в процессах.
          </p>
        </div>

        <div class="mt-14 max-w-3xl">
          <!-- Dori.pro -->
          <article class="flex flex-col rounded-[24px] border border-line bg-white p-6 transition-colors hover:border-line-strong sm:p-8">
            <div class="flex items-start justify-between gap-4">
              <div class="flex h-11 w-11 items-center justify-center rounded-xl border border-line bg-canvas text-sage">
                <svg class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M10.5 20.5a4.95 4.95 0 0 1-7-7l10-10a4.95 4.95 0 0 1 7 7l-10 10Z" />
                  <path d="M8.5 8.5l7 7" />
                </svg>
              </div>
              <span class="rounded-full border border-sage/20 bg-sage/5 px-3 py-1 text-[10px] font-medium uppercase tracking-[0.12em] text-sage">
                Фармацевтика & Ритейл
              </span>
            </div>

            <h3 class="mt-6 text-2xl font-medium tracking-tight text-ink">
              Dori.pro
              <span class="mt-1 block text-base font-normal text-muted">Управление аптечной сетью</span>
            </h3>

            <p class="mt-4 text-sm leading-relaxed text-muted">
              В отличие от тяжелых 1С, SAP или стандартных WMS, Dori.pro — это адаптивная система, которая в реальном времени автоматически планирует закупки и распределяет товар от склада до касс, предотвращая ошибки персонала. Не требует долгих месяцев обучения.
            </p>

            <ul class="mt-6 space-y-3 border-t border-line pt-6">
              <li class="flex gap-3 text-sm leading-relaxed text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-sage/10 text-sage">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Автораспределение товара с учетом спроса и сроков (FEFO)
              </li>
              <li class="flex gap-3 text-sm leading-relaxed text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-sage/10 text-sage">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Защита от ошибок неквалифицированного персонала
              </li>
              <li class="flex gap-3 text-sm leading-relaxed text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-sage/10 text-sage">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Пересчет остатков в реальном времени и выявление аномалий
              </li>
            </ul>

            <div class="mt-auto pt-8">
              <button
                type="button"
                class="inline-flex items-center gap-2 rounded-full bg-ink px-5 py-3 text-sm font-medium text-white transition-colors hover:bg-sage"
                @click="openProject('dori')"
              >
                Подробнее о системе
                <svg class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14M13 6l6 6-6 6" /></svg>
              </button>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- Преимущества -->
    <section id="features" class="border-b border-line bg-canvas py-24 lg:py-32">
      <div class="site-container">
        <div class="grid gap-8 lg:grid-cols-2 lg:items-end">
          <div>
            <p class="text-[11px] font-semibold uppercase tracking-[0.18em] text-brand">Преимущества</p>
            <h2 class="mt-4 max-w-xl text-3xl font-medium leading-[1.05] tracking-[-0.035em] text-ink sm:text-4xl lg:text-5xl">
              Почему бизнес<br />выбирает нас
            </h2>
          </div>
          <p class="max-w-xl text-sm leading-relaxed text-muted lg:justify-self-end">
            Мы убираем человеческий фактор и рутину, оставляя бизнесу прозрачные процессы, точные данные и время на принятие решений.
          </p>
        </div>

        <div class="mt-14 grid gap-4 md:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="feature in features"
            :key="feature.title"
            class="surface-card p-7 transition-colors hover:border-line-strong"
          >
            <div
              class="flex h-11 w-11 items-center justify-center rounded-xl"
              :class="feature.accent === 'sage' ? 'bg-sage/10 text-sage' : 'bg-brand-soft text-brand'"
            >
              <svg class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
                <path :d="feature.icon" />
              </svg>
            </div>
            <h3 class="mt-6 text-base font-semibold text-ink">{{ feature.title }}</h3>
            <p class="mt-3 text-sm leading-relaxed text-muted">{{ feature.text }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- Проблемы и решения -->
    <section id="problems" class="bg-white py-24 lg:py-32">
      <div class="site-container">
        <div class="grid gap-8 lg:grid-cols-2 lg:items-end">
          <div>
            <p class="text-[11px] font-semibold uppercase tracking-[0.18em] text-brand">Проблемы и решения</p>
            <h2 class="mt-4 max-w-xl text-3xl font-medium leading-[1.05] tracking-[-0.035em] text-ink sm:text-4xl lg:text-5xl">
              С чем сталкивается<br />бизнес
            </h2>
          </div>
          <p class="max-w-xl text-sm leading-relaxed text-muted lg:justify-self-end">
            Мы устраняем хаос, человеческий фактор и финансовые потери на всех уровнях: от закупа до касс.
          </p>
        </div>

        <div class="mt-14 grid gap-4 lg:grid-cols-2">
          <article
            v-for="(problem, index) in problems"
            :key="problem.title"
            class="flex flex-col rounded-[20px] border border-line bg-white p-6 transition-colors hover:border-line-strong sm:p-7"
          >
            <div class="flex items-center justify-between gap-4">
              <span class="text-[11px] font-medium uppercase tracking-[0.16em] text-muted">{{ problem.title }}</span>
              <span class="text-[11px] tabular-nums text-soft">{{ String(index + 1).padStart(2, '0') }}</span>
            </div>

            <p class="mt-5 text-[15px] leading-relaxed text-ink/80">«{{ problem.quote }}»</p>

            <div class="mt-auto pt-6">
              <div class="border-t border-line pt-5">
                <div class="flex items-center gap-2 text-[11px] font-medium uppercase tracking-[0.16em] text-brand">
                  <span class="h-1.5 w-1.5 rounded-full bg-brand"></span>
                  Наше решение
                </div>
                <p class="mt-3 text-sm leading-relaxed text-muted">{{ problem.solution }}</p>
              </div>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- Контакты -->
    <section id="contact" class="border-y border-line bg-canvas py-24 lg:py-32">
      <div class="site-container">
        <div class="grid gap-12 lg:grid-cols-[0.85fr_1.15fr] lg:gap-20">
          <div class="self-start lg:sticky lg:top-28">
            <p class="text-[11px] font-semibold uppercase tracking-[0.18em] text-brand">Контакты</p>
            <h2 class="mt-4 text-3xl font-medium leading-[1.05] tracking-[-0.035em] text-ink sm:text-4xl lg:text-5xl">
              Обсудим<br />ваш проект?
            </h2>
            <p class="mt-5 max-w-md text-sm leading-relaxed text-muted">
              Оставьте заявку, и наш архитектор свяжется с вами для бесплатной консультации. Разберём процессы и предложим решение под вашу задачу.
            </p>

            <ul class="mt-8 space-y-4 border-t border-line pt-8">
              <li class="flex items-start gap-3 text-sm text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-brand-soft text-brand">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Бесплатная консультация с архитектором
              </li>
              <li class="flex items-start gap-3 text-sm text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-brand-soft text-brand">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Ответ в течение рабочего дня
              </li>
              <li class="flex items-start gap-3 text-sm text-muted">
                <span class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full bg-brand-soft text-brand">
                  <svg class="h-3 w-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
                </span>
                Подбор продукта и плана внедрения
              </li>
            </ul>

            <div class="mt-8 rounded-2xl border border-line bg-white p-5">
              <p class="text-[10px] font-medium uppercase tracking-[0.16em] text-muted">Формат встречи</p>
              <p class="mt-2 text-sm leading-relaxed text-ink">Онлайн-звонок · 30 минут · без обязательств</p>
            </div>
          </div>

          <div class="rounded-[24px] border border-line bg-white p-6 sm:p-8 lg:p-10">
            <div v-if="isSubmitted" class="flex min-h-[420px] flex-col items-center justify-center text-center">
              <span class="flex h-14 w-14 items-center justify-center rounded-full bg-sage/10 text-sage">
                <svg class="h-7 w-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12.5l4.5 4.5L19 7" /></svg>
              </span>
              <h3 class="mt-6 text-xl font-medium tracking-tight text-ink">Заявка успешно отправлена!</h3>
              <p class="mt-3 max-w-sm text-sm leading-relaxed text-muted">Мы свяжемся с вами в течение рабочего дня.</p>
              <button
                type="button"
                class="mt-8 inline-flex items-center justify-center rounded-full border border-line px-5 py-3 text-sm font-medium text-ink transition hover:border-ink"
                @click="isSubmitted = false"
              >
                Отправить ещё одну заявку
              </button>
            </div>

            <form v-else class="space-y-6" @submit.prevent="submitForm">
              <div class="grid gap-6 sm:grid-cols-2">
                <div>
                  <label for="name" class="mb-2 block text-[11px] font-semibold uppercase tracking-[0.14em] text-muted">Ваше имя *</label>
                  <input id="name" v-model="form.name" type="text" required autocomplete="name" placeholder="Имя" class="field" />
                </div>
                <div>
                  <label for="phone" class="mb-2 block text-[11px] font-semibold uppercase tracking-[0.14em] text-muted">Телефон *</label>
                  <input id="phone" v-model="form.phone" type="tel" required autocomplete="tel" placeholder="+998 (__) ___-__-__" class="field" />
                </div>
              </div>

              <div>
                <label for="company" class="mb-2 block text-[11px] font-semibold uppercase tracking-[0.14em] text-muted">Название компании</label>
                <input id="company" v-model="form.company" type="text" autocomplete="organization" placeholder="Компания" class="field" />
              </div>

              <div>
                <label for="message" class="mb-2 block text-[11px] font-semibold uppercase tracking-[0.14em] text-muted">Ваш вопрос</label>
                <textarea id="message" v-model="form.message" rows="4" placeholder="Коротко опишите задачу" class="field resize-none"></textarea>
              </div>

              <button
                type="submit"
                :disabled="isSubmitting"
                class="w-full rounded-full bg-ink py-4 text-sm font-medium text-white transition-colors hover:bg-brand disabled:cursor-not-allowed disabled:opacity-60"
              >
                {{ isSubmitting ? 'Отправка...' : 'Отправить заявку' }}
              </button>

              <p v-if="submitError" class="rounded-xl border border-rose-200 bg-rose-50 p-3 text-center text-sm text-rose-700">
                Не удалось отправить заявку. Попробуйте ещё раз или напишите нам на почту.
              </p>

              <p class="text-center text-[11px] leading-relaxed text-soft">
                Нажимая кнопку, вы соглашаетесь на обработку персональных данных.
              </p>
            </form>
          </div>
        </div>
      </div>
    </section>

    <!-- Подвал -->
    <footer class="bg-white py-10">
      <div class="site-container flex flex-col gap-6 sm:flex-row sm:items-center sm:justify-between">
        <div class="flex items-center gap-3">
          <span class="flex h-8 w-8 items-center justify-center rounded-lg bg-ink text-xs font-semibold text-white">D</span>
          <span class="text-sm font-semibold tracking-tight text-ink">Dori<span class="text-brand">.pro</span></span>
        </div>

        <nav class="flex flex-wrap items-center gap-x-6 gap-y-2 text-sm text-muted">
          <a href="#products" class="transition hover:text-ink">О системе</a>
          <a href="#features" class="transition hover:text-ink">Преимущества</a>
          <a href="#problems" class="transition hover:text-ink">Решения</a>
          <a href="#contact" class="transition hover:text-ink">Контакты</a>
        </nav>

        <p class="text-xs text-soft">© 2026 Dori.pro. Все права защищены.</p>
      </div>
    </footer>

    <!-- Модальное окно с деталями проекта -->
    <div
      v-if="activeProject"
      class="fixed inset-0 z-50 flex items-end justify-center bg-ink/50 p-0 backdrop-blur-sm sm:items-center sm:p-4"
      @click.self="closeProject"
    >
      <div class="flex max-h-[92vh] w-full max-w-6xl flex-col overflow-hidden rounded-t-[24px] border border-line bg-white shadow-2xl sm:max-h-[90vh] sm:rounded-[24px]">
        <div class="flex shrink-0 items-center justify-between border-b border-line bg-white px-5 py-4 sm:px-8">
          <span class="text-[11px] font-semibold uppercase tracking-[0.16em] text-muted">{{ projectLabel }}</span>
          <button
            type="button"
            class="flex h-10 w-10 items-center justify-center rounded-full border border-line text-muted transition hover:bg-canvas hover:text-ink"
            aria-label="Закрыть"
            @click="closeProject"
          >
            <svg class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round">
              <path d="M6 6l12 12M18 6L6 18" />
            </svg>
          </button>
        </div>

        <div class="min-h-0 flex-1 overflow-y-auto px-5 py-6 sm:px-8 sm:py-8 lg:px-10">
          <!-- ===================== DORI.PRO ===================== -->
          <div v-if="activeProject === 'dori'">
            <h2 class="text-2xl font-medium tracking-tight text-ink lg:text-3xl">Dori.pro — система управления аптечной сетью</h2>
            <p class="mt-3 max-w-4xl text-sm leading-relaxed text-muted">
              Единая система управления аптечной сетью, объединяющая работу кассира, склада, закупок, логистики и руководства в одном цифровом контуре.
              Все данные — остатки, продажи, движения товаров, платежи — хранятся в облаке и доступны в реальном времени: от кассы до рабочего места директора.
            </p>

            <div class="mt-8 border-t border-line">
              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Проблема и решение</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <div class="mt-4 grid gap-3 text-sm sm:grid-cols-2">
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-ink">Без единой системы</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Продажи и остатки «на бумаге», закупки «вслепую», распределение товара вручную, нет контроля сроков годности, отчёты собираются с опозданием.</p>
                  </div>
                  <div class="rounded-xl border border-sage/20 bg-sage/5 p-4">
                    <p class="font-semibold text-sage">С Dori.pro</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Каждое движение товара фиксируется автоматически. Потребность аптек рассчитывается по фактическим продажам, а товар распределяется сам с учётом спроса и сроков годности.</p>
                  </div>
                </div>
              </details>

              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Ключевое преимущество: анализ потребностей и автораспределение</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <div class="mt-4 space-y-3 text-sm">
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Анализ потребностей аптек</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Расчёт потребности каждой аптеки по истории продаж за 90 дней: прогноз спроса на 7 / 14 / 21 день («горит» / «нужно» / «можно»), потребность на 28 дней, дефицит и избыток, автоматический объём заказа у поставщика с учётом остатков и категории товара.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Автоматическое распределение по аптекам</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Система сама предлагает, кому и сколько отгрузить: три уровня приоритета, учёт сроков годности, бережное отношение к партиям, исключение повторного распределения, приоритизация аптек с высоким спросом. По результатам — готовые листы отгрузки и задачи для сборщиков.</p>
                  </div>
                </div>
              </details>

              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Возможности по модулям</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <div class="mt-4 grid gap-3 text-sm sm:grid-cols-2">
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">POS-касса (продажи)</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Быстрая продажа по штрихкоду, списание по FEFO, продажа в долг, карантин товара, смешанные платежи (наличные + карта + QR), печать чека 58 мм, X/Z-отчёты и закрытие бизнес-дня.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Платёжные интеграции</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Фискализация через Arca (UzCard), карты Humo/UzCard/SmartOne, QR-платежи Click, Payme, Uzum FastPay. Устойчивость к сбоям без двойного списания (RRN).</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Склад и запасы</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Приёмка по партиям, автоматическое распределение и перемещения, ячейки/полки, инвентаризации, контроль отрицательных остатков и сроков годности, полный журнал движений.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Заказы и закупки</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Заявки аптек, автоматический расчёт объёма к заказу, заказы поставщикам, договоры и счета, сравнение накладной и поступления, анализ поставщиков по срокам, ценам и KPI.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Логистика и мобильные приложения</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Сборка и упаковка по листам распределения, отслеживание посылок, приложение водителя, приложение менеджера аптеки: распределение по ячейкам со сканированием камерой.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Отчёты и аналитика</p>
                    <p class="mt-1.5 leading-relaxed text-muted">KPI по закупкам, продажам, контролёрам, сборщикам и поставщикам. Отчёты по товарообороту, остаткам, продажам по фармацевтам и категориям, прогноз спроса, бонусы и дашборд руководства.</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Задачи и процессы</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Housekeeping/задачи, проекты и шаблоны типовых процессов, маркетинг (договоры, план продаж, акции и скидки).</p>
                  </div>
                  <div class="rounded-xl border border-line bg-canvas p-4">
                    <p class="font-semibold text-sage">Контроль GPP и справочники</p>
                    <p class="mt-1.5 leading-relaxed text-muted">Журнал замеров температуры в зонах хранения, единый товарный каталог, справочники, роли и права доступа, настройка состава сети и терминалов.</p>
                  </div>
                </div>
              </details>

              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Как это устроено</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <ul class="mt-3 list-disc space-y-1.5 pl-5 text-sm leading-relaxed text-muted marker:text-sage">
                  <li>Облачная база данных (HA / PostgreSQL / Supabase) с резервным копированием.</li>
                  <li>Работа в реальном времени и при слабой связи: касса работает даже при сбоях интернета.</li>
                  <li>Безопасность по ролям и шифрование данных, автоматические удалённые обновления.</li>
                  <li>Поддержка оборудования: штрих-сканеры, термопринтеры 58 мм, фискальные и банковские терминалы, весовые платформы.</li>
                </ul>
              </details>

              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Выгоды для бизнеса</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <ul class="mt-3 list-disc space-y-1.5 pl-5 text-sm leading-relaxed text-muted marker:text-sage">
                  <li>Скорость кассы — меньше очередей, больше оборот.</li>
                  <li>Точные остатки в любой момент по любой аптеке — минимум потерь.</li>
                  <li>Умное распределение товара — товар попадает туда, где его ждут.</li>
                  <li>Меньше списаний благодаря распределению по срокам годности.</li>
                  <li>Прозрачная отчётность и единые процессы для всех точек сети.</li>
                </ul>
              </details>

              <details class="group border-b border-line py-5" data-accordion>
                <summary class="flex cursor-pointer list-none items-center justify-between gap-3 sm:pointer-events-none [&::-webkit-details-marker]:hidden">
                  <h4 class="text-sm font-semibold text-ink sm:text-base">Внедрение и поддержка</h4>
                  <svg class="h-4 w-4 shrink-0 text-muted transition-transform group-open:rotate-180 sm:hidden" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6" /></svg>
                </summary>
                <p class="mt-3 text-sm leading-relaxed text-muted">
                  Анализ процессов и настройка под нужды бизнеса, оборудование касс и терминалов, обучение персонала и дальнейшее сопровождение:
                  обновления, обучение новых сотрудников, добавление функционала по мере роста.
                </p>
              </details>
            </div>
          </div>

          <button
            type="button"
            class="mt-10 hidden w-full rounded-full bg-ink py-3.5 text-sm font-medium text-white transition-colors hover:bg-brand sm:block"
            @click="closeProject"
          >
            Закрыть
          </button>
        </div>
      </div>
    </div>

  </div>
</template>
