<script setup lang="ts">
const route = useRoute();

const links = [
    {
        label: "Home",
        icon: "i-heroicons-home",
        to: "/",
    },
    {
        label: "Bed overview",
        icon: "i-heroicons-home",
        to: "/beds/beds",
    },
    {
        label: "Robot status",
        icon: "i-heroicons-chart-bar",
        to: "/robots/robots",
    },
    {
        label: "Reports",
        icon: "i-heroicons-command-line",
        to: "/reports/reports",
    },
];
</script>

<template>
    <UDashboardPage>
        <UDashboardPanel :width="300" :resizable="{ min: 200, max: 400 }">
            <UDashboardNavbar>
                <template #left>
                    <Logo class="w-auto h-5" />
                </template>
            </UDashboardNavbar>

            <UDashboardSidebar>
                <UDashboardSidebarLinks :links="links" />
            </UDashboardSidebar>
        </UDashboardPanel>
    </UDashboardPage>
</template>
