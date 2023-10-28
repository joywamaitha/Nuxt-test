<template>
    <div class="bg-gray-100 min-h-screen flex items-center justify-center">
        <div class="max-w-md p-6 mx-auto bg-white rounded-lg shadow-md">
            <h1 class="text-2xl font-semibold mb-4">Random Data Generator</h1>
            <button @click="fetchRandomData"
                class="bg-blue-500 hover:bg-blue-600 text-white font-semibold px-4 py-2 rounded-lg">
                Fetch Random Data
            </button>
            <div v-if="formattedData" class="mt-4">
                <h2 class="text-lg font-semibold mb-2">Random Data:</h2>
                <div class="bg-gray-200 p-4 rounded-lg formatted-data" ref="jsonFormatter">
                    <p>
                        <strong>First Name:</strong>
                        <span class="word">
                            <span v-for="(letter, index) in firstName" :key="index" class="animate-box">
                                {{ letter }}
                            </span>
                        </span>
                    </p>
                    <p>
                        <strong>Last Name:</strong>
                        <span class="word animate-spin">{{ lastName }}</span>
                    </p>
                    <p>
                        <strong>Address:</strong>
                        <span class="word">
                            <span v-for="(letter, index) in address" :key="index" class="animate-box">
                                {{ letter }}
                            </span>
                        </span>
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            formattedData: null,
            firstName: '',
            lastName: '',
            address: '',
        };
    },
    methods: {
        async fetchRandomData() {
            try {
                const response = await fetch('https://random-data-api.com/api/users/random_user');
                const data = await response.json();

                // Extract specific fields
                this.firstName = data.first_name;
                this.lastName = data.last_name;
                this.address = `${data.address.street_address}, ${data.address.city}, ${data.address.state}`;

                // Format the JSON data for better display
                this.formattedData = JSON.stringify(
                    { 'First Name': this.firstName, 'Last Name': this.lastName, 'Address': this.address },
                    null,
                    2
                );
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        },
    },
};
</script>
  
<style scoped>
.formatted-data {
    font-family: 'Courier New', monospace;
    font-size: 18px;
}

/* CSS for individual letters and effects */
.word {
    display: inline-block;
}

.animate-spin {
    animation: spin 2s infinite linear;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

.animate-box {
    display: inline-block;
    padding: 5px;
    margin: 2px;
    font-size: 24px;
    animation: zoom 1s alternate infinite, color-change 5s alternate infinite;
    background-color: #ff0000;
    /* Initial color (red) */
    border-radius: 10px;
    box-shadow: 20px 10px 15px rgba(7, 7, 7, 0.2);
}

@keyframes zoom {
    0% {
        transform: scale(1);
    }

    100% {
        transform: scale(1.1);
    }
}

@keyframes color-change {
    0% {
        background-color: rgb(252, 92, 92);
    }

    20% {
        background-color: rgb(100, 100, 247);
    }

    40% {
        background-color: rgb(49, 211, 240);
    }

    60% {
        background-color: rgb(245, 90, 245);
    }

    80% {
        background-color: lightgreen;
    }

    100% {
        background-color: rgb(255, 136, 0);
    }
}
</style>
  
  
  
  
  
  