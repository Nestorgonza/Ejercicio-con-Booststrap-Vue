<template>
    <div class="mt-5">
        <h1>Vuelidate</h1>
        <form @submit.prevent="submit">
            <input type="email" placeholder="Ingresa un email" class="form-control my-3"
                v-model.lazy="$v.email.$model"
                :class="{'is-invalid': $v.email.$error}">
            <p class="text-danger" v-if="!$v.email.email">Este email es incorrecto</p>
            <p class="text-danger" v-if="!$v.email.required">Este campo es requerido</p>
            <!-- <p>{{$v.email}}</p> -->
            <input type="password" placeholder="Ingresa contraseña" class="form-control my-3"
                v-model.lazy="$v.password.$model"
                :class="{'is-invalid': $v.password.$error}">
            <p class="text-danger" v-if="!$v.password.minLength">Minimo 6 carácteres</p>            
            <p class="text-danger" v-if="!$v.password.required">Este campo es requerido</p>
            <!-- <p>{{$v.password}}</p> -->
            <input type="password" placeholder="Repite contraseña" class="form-control my-3"
                v-model.lazy="$v.repeatPassword.$model"
                :class="{'is-invalid': $v.repeatPassword.$error}"> 
            <p class="text-danger" v-if="!$v.repeatPassword.sameAsPassword">Contraseña incorrecta</p>               
            <!-- <p>{{$v.repeatPassword}}</p>-->
            <b-button variant="outline-primary" type="submit"
                :disabled="$v.$invalid">Enviar</b-button>
            <p>{{$v.$invalid}}</p>
            <p>{{$v}}</p>
        </form>
    </div>
</template>

<script>
import { required, email, sameAs, minLength } from 'vuelidate/lib/validators';

export default {
    name: 'Validaciones',
    data() {
        return {
            email: '',
            password: '',
            repeatPassword: ''
        }
    },
    validations: {
        email: {required, email},
        password: {
            required,
            minLength: minLength(6)
        },
        repeatPassword: {
            sameAsPassword: sameAs('password')
        }
    }, 
    methods: {
        submit() {
            console.log('submit!')
            this.$v.$touch()
            if (this.$v.$invalid) {
                // this.submitStatus = 'ERROR'
                console.log('Se generó un error')
            } else {
                console.log('Todos los campos correctos')
                console.log('Enviando información...' + this.$v.email.$model + ' ' +
                this.$v.password.$model)
            }
        }
    }
}
</script>