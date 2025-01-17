<template>
    <section class="relative flex flex-col items-center justify-center min-h-screen bg-gray-100">
        <Card v-if="!showAlert" class="w-1/2">
            <CardHeader>
                <CardTitle class="text-2xl font-bold text-center text-gray-800 mb-4">Contactez-nous</CardTitle>
            </CardHeader>

            <CardContent>
                <form @submit.prevent="sendMail" class="gap-4 flex flex-col">
                    <FormField v-slot="{ componentField }" name="username">
                        <FormItem>
                            <FormLabel>Votre nom</FormLabel>
                            <FormControl>
                                <Input type="text" placeholder="Entrez votre nom" v-bind="componentField" />
                            </FormControl>
                            <FormMessage />
                        </FormItem>
                    </FormField>

                    <FormField v-slot="{ componentField }" name="email">
                        <FormItem>
                            <FormLabel>Votre email</FormLabel>
                            <FormControl>
                                <Input type="text" placeholder="Entrez votre email" v-bind="componentField" />
                            </FormControl>
                            <FormMessage />
                        </FormItem>
                    </FormField>

                    <FormField v-slot="{ componentField }" name="sujet">
                        <FormItem>
                            <FormLabel>Le sujet</FormLabel>
                            <FormControl>
                                <Input type="text" placeholder="Entrez le sujet" v-bind="componentField" />
                            </FormControl>
                            <FormMessage />
                        </FormItem>
                    </FormField>

                    <FormField v-slot="{ componentField }" name="message">
                        <FormItem>
                            <FormLabel>Votre message</FormLabel>
                            <FormControl>
                                <Textarea placeholder="Entrez votre message" v-bind="componentField" />
                            </FormControl>
                            <FormMessage />
                        </FormItem>
                    </FormField>

                    <Button type="submit" class="w-full text-white bg-indigo-600">
                        Envoyer
                    </Button>
                </form>
            </CardContent>
        </Card>

        <!-- Alert Overlay -->
        <div v-if="showAlert" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
            <Card class="w-96 p-6 bg-white rounded-md shadow-lg">
                <Alert>
                    <AlertTitle>Message envoyé avec succès</AlertTitle>
                    <AlertDescription>
                        <p>Nom : {{ payload.username }}</p>
                        <p>Email : {{ payload.email }}</p>
                        <p>Sujet : {{ payload.sujet }}</p>
                        <p>Message : {{ payload.message }}</p>
                    </AlertDescription>
                </Alert>
                <Button class="mt-4 w-full py-2 px-4 text-white bg-indigo-600 rounded-md" @click="closeAlert">
                    OK
                </Button>
            </Card>
        </div>
    </section>
</template>

<script setup lang="ts">
import { useForm } from 'vee-validate'
import { toTypedSchema } from '@vee-validate/zod'
import * as z from 'zod'

const formSchema = toTypedSchema(z.object({
    username: z.string().min(2).max(50),
    email: z.string().email(),
    sujet: z.string().min(2),
    message: z.string().min(2),
}))

const form = useForm({
    validationSchema: formSchema,
})

const payload = reactive({
    username: "",
    email: "",
    sujet: "",
    message: "",
})
const showAlert = ref(false)

const sendMail = form.handleSubmit((values) => {
    Object.assign(payload, values)
    // Show the alert overlay
    showAlert.value = true
})

function closeAlert() {
    // Hide the alert overlay
    showAlert.value = false

    // Reset form fields after dismissing the alert
    Object.assign(payload, {
        username: "",
        email: "",
        sujet: "",
        message: "",
    })
}
</script>
