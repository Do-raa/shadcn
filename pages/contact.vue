<template>
    <section class="relative flex flex-col items-center justify-center min-h-screen bg-gray-100">
        <Card v-if="!showAlert">
            <CardHeader>
                <CardTitle class="text-2xl font-bold text-center text-gray-800 mb-4">Contactez-nous</CardTitle>
            </CardHeader>
            <CardContent>
                <form @submit.prevent="sendMail">
                    <div class="mb-4">
                        <Label for="sujet" class="block text-sm font-medium text-gray-700 mb-2">Sujet</Label>
                        <Input id="sujet" v-model="sujet" type="text"
                            class="block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500"
                            placeholder="Entrez le sujet" required />
                    </div>
                    <div class="mb-4">
                        <Label for="message" class="block text-sm font-medium text-gray-700 mb-2">Message</Label>
                        <Textarea id="message" v-model="message" rows="4"
                            class="block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500"
                            placeholder="Entrez votre message" required></Textarea>
                    </div>
                    <Button type="submit"
                        class="w-full py-2 px-4 text-white bg-indigo-600 rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
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
                        <p>Sujet : {{ sujet }}</p>
                        <p>Message : {{ message }}</p>
                    </AlertDescription>
                </Alert>
                <Button
                    class="mt-4 w-full py-2 px-4 text-white bg-indigo-600 rounded-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                    @click="closeAlert">
                    OK
                </Button>
            </Card>
        </div>
    </section>
</template>

---

### Script

```javascript
<script>
import { Button } from '@/components/ui/button';
import { Label } from '@/components/ui/label';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import {
    Card,
    CardContent,
    CardHeader,
    CardTitle,
} from '@/components/ui/card';
import { Alert, AlertTitle, AlertDescription } from '@/components/ui/alert';

export default {
    data() {
        return {
            sujet: '',
            message: '',
            showAlert: false,
        };
    },
    components: {
        Card,
        CardContent,
        CardHeader,
        CardTitle,
        Button,
        Label,
        Input,
        Textarea,
        Alert,
        AlertTitle,
        AlertDescription,
    },
    methods: {
        sendMail() {
            // Show the alert overlay
            this.showAlert = true;
        },
        closeAlert() {
            // Hide the alert overlay
            this.showAlert = false;

            // Reset form fields after dismissing the alert
            this.sujet = '';
            this.message = '';
        },
    },
};
</script>
