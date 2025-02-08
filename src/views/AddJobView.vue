<script setup>
import router from "@/router";
import { reactive } from "vue";
import {useToast} from "vue-toastification";
import axios from "axios";

// Reactive form data
const form = reactive({
    type: "Full-Time",
    title: "",
    description: "",
    salary: "",
    location: "",
    company: {
        name: "",
        description: "",
        contactEmail: "",
        contactPhone: "",
    },
});

const toast = useToast();

const handleSubmit = async() => {
    const newJob = {
        title: form.title,
        type: form.type,
        location: form.location,
        description: form.description,
        salary: form.salary,
        company: {
            name: form.company.name,
            description: form.company.description,
            contactEmail: form.company.contactEmail,
            contactPhone: form.company.contactPhone,
        },
    };
    try {
        const response = await axios.post('/api/jobs/', newJob);
        toast.success("Job added successfully");
        router.push(`/jobs/${response.data.id}`);
    } catch (error) {
        console.error("Error fetching job", error);
        toast.error("Job was not added. Please try again.");
    }
};
</script>

<template>
    <section class="bg-green-50">
        <div class="container m-auto max-w-2xl py-24">
            <div
                class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
                <form @submit.prevent="handleSubmit">
                    <h2 class="text-3xl text-center font-semibold mb-6">
                        Add Job
                    </h2>

                    <!-- Job Type -->
                    <div class="mb-4">
                        <label
                            for="type"
                            class="block text-gray-700 font-bold mb-2"
                            >Job Type</label>
                        <select
                            id="type"
                            v-model="form.type"
                            name="type"
                            class="border rounded w-full py-2 px-3"
                            required>
                            <option value="Full-Time">Full-Time</option>
                            <option value="Part-Time">Part-Time</option>
                            <option value="Remote">Remote</option>
                            <option value="Internship">Internship</option>
                        </select>
                    </div>

                    <!-- Job Title -->
                    <div class="mb-4">
                        <label class="block text-gray-700 font-bold mb-2"
                            >Job Listing Name</label
                        >
                        <input
                            type="text"
                            v-model="form.title"
                            id="name"
                            name="name"
                            class="border rounded w-full py-2 px-3 mb-2"
                            placeholder="Job Title"
                            required
                        />
                    </div>

                    <!-- Description -->
                    <div class="mb-4">
                        <label
                            for="description"
                            class="block text-gray-700 font-bold mb-2"
                            >Description</label
                        >
                        <textarea
                            id="description"
                            v-model="form.description"
                            name="description"
                            class="border rounded w-full py-2 px-3"
                            rows="4"
                            placeholder="Job Description"
                            required
                        ></textarea>
                    </div>

                    <!-- Salary -->
                    <div class="mb-4">
                        <label
                            for="salary"
                            class="block text-gray-700 font-bold mb-2"
                            >Salary</label
                        >
                        <select
                            id="salary"
                            v-model="form.salary"
                            name="salary"
                            class="border rounded w-full py-2 px-3"
                        >
                            <option value="Under $50K">Under $50K</option>
                            <option value="$50K - $60K">$50K - $60K</option>
                            <option value="$60K - $70K">$60K - $70K</option>
                            <option value="Over $200K">Over $200K</option>
                        </select>
                    </div>

                    <!-- Location -->
                    <div class="mb-4">
                        <label
                            for="location"
                            class="block text-gray-700 font-bold mb-2"
                            >Location</label
                        >
                        <input
                            type="text"
                            v-model="form.location"
                            id="location"
                            name="location"
                            class="border rounded w-full py-2 px-3 mb-2"
                            placeholder="Location"
                            required
                        />
                    </div>

                    <!-- Company Info -->
                    <div class="mb-4">
                        <label
                            for="company_name"
                            class="block text-gray-700 font-bold mb-2"
                            >Company Name</label
                        >
                        <input
                            type="text"
                            v-model="form.company.name"
                            id="company_name"
                            name="company_name"
                            class="border rounded w-full py-2 px-3"
                            placeholder="Company Name"
                        />
                    </div>

                    <div class="mb-4">
                        <label
                            for="company_description"
                            class="block text-gray-700 font-bold mb-2"
                            >Company Description</label>
                        <textarea
                        id="company_description"
                        v-model="form.company.description"
                            name="company_description"
                            class="border rounded w-full py-2 px-3"
                            rows="4"
                            placeholder="What does the company do?">
                        </textarea>
                    </div>

                    <div class="mb-4">
                        <label
                            for="company_email"
                            class="block text-gray-700 font-bold mb-2"
                            >Company Email</label>
                        <input
                            type="email"
                            v-model="form.company.contactEmail"
                            id="company_email"
                            name="company_email"
                            class="border rounded w-full py-2 px-3"
                            placeholder="Email address for application"/>
                    </div>
                    <div class="mb-4">
                        <label
                            for="company_phone"
                            class="block text-gray-700 font-bold mb-2"
                            >Company Phone</label>
                        <input
                            type="tel"
                            v-model="form.company.contactPhone"
                            id="company_phone"
                            name="company_phone"
                            class="border rounded w-full py-2 px-3"
                            placeholder="Optional phone for application"/>
                    </div>
                    <div class="mt-6">
                        <button
                        class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
                        type="submit">
                        Add Job
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </section>
</template>
