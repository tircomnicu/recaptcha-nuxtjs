<template>
  <div class="form-wrapper">
    <h2 class="form__title">reCaptcha v2</h2>
    <p class="form__subtitle">
      Don't forget to add your site key to nuxt.config.js file
    </p>
    <form
      ref="form"
      class="form"
      action="YOUR ACTION"
      method="post"
      @submit.prevent="onSubmit"
    >
      <fieldset>
        <input type="text" name="name" placeholder="Name" />
        <input placeholder="Email" type="email" name="email" />
      </fieldset>
      <recaptcha @error="onError" @success="onSuccess" @expired="onExpired" />
      <button type="submit">Submit now</button>
    </form>
  </div>
</template>

<script>
export default {
  methods: {
    onError(error) {
      // eslint-disable-next-line no-console
      console.log('Error happened:', error)
    },
    async onSubmit() {
      try {
        const token = await this.$recaptcha.getResponse()
        // eslint-disable-next-line no-console
        console.log('ReCaptcha token:', token)
        await this.$recaptcha.reset()
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log('Login error:', error)
      }
    },
    onSuccess(token) {
      // eslint-disable-next-line no-console
      console.log('Succeeded:', token)
      this.$refs.form.submit()
    },
    onExpired() {
      // eslint-disable-next-line no-console
      console.log('Expired')
    }
  }
}
</script>

<style lang="sass" scoped>
.form-wrapper
  padding: 0 2.4rem
  @media (min-width: 45em)
    box-shadow: $shadow
    border-radius: $br
    max-width: 64rem
    margin: 0 auto
    padding: 4.8rem

.form
  &__title
    font-size: 3.2rem
    font-weight: $medium
    line-height: 3.2rem
  &__subtitle
    font-size: 1.6rem
    line-height: 2.4rem
    padding-top: 1.6rem
  padding: 4.8rem 0 2.4rem
  & fieldset
    border: none
  & input
    display: block
    border: none
    margin-bottom: 2.4rem
    font-size: 1.6rem
    width: 100%
    padding: .8rem 0
    border-bottom: .1rem solid $text_base
    &:focus
      outline: none
      border-color: $primary
  & button
    background-color: $primary
    border-radius: 60rem
    box-shadow: .4rem .4rem 1.6rem 0 rgba(0,0,0,.16)
    color: $text_inverse
    font-size: 1.6rem
    font-weight: $medium
    padding: 1.6rem 3.4rem
    margin-top: 2.4rem
    max-width: 24rem
    text-transform: uppercase
    transition: all .25s ease-in-out
    width: 100%
    @media (max-width: 46em)
      padding: 1.6rem 2.4rem
    &:hover,
    &:active
      background-color: $secondary
      box-shadow: .2rem .2rem .8rem 0 rgba(0,0,0,.16)
</style>
