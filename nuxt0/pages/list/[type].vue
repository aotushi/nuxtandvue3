<!-- 课程列表页 -->
<script setup lang="ts">
import type { IResult } from "~/types/IResult";

const route = useRoute();
const type = route.params.type as string;
const title = type === "course" ? "课程" : "专栏";
useHead({ title });

const url = type === "course" ? "/api/course" : "/api/column";
const page = ref(1);
const size = ref(8);
const { data, pending } = await useFetch<IResult>(() => `${url}?page=${page.value - 1}`, {
  watch: [page],
});

const onPageChange = (pageNum) => {
  page.value = pageNum;
};
</script>

<template>
  <div>
    <NBreadcrumb class="mb-5">
      <NBreadcrumbItem>
        <nuxt-link to="/"> 首页 </nuxt-link>
      </NBreadcrumbItem>
      <NBreadcrumbItem>
        {{ title }}
      </NBreadcrumbItem>
    </NBreadcrumb>
    <!-- 课程渲染 -->
    <Loading :pending="pending">
      <template #loading> <LoadingCourseSkeleton /></template>
      <template #default>
        <NGrid :x-gap="20" responsive="screen" cols="1 m:4" class="mb-6">
          <NGi v-for="item in data?.data.list" :key="item.id">
            <Prod :data="item" :type="type" />
          </NGi>
        </NGrid>
        <!-- 分页组件 -->
        <div class="flex justify-center items-center mt-5 mb-10">
          <NPagination size="large" :item-count="data?.data.total" :page="page" :page-size="size" :on-update:page="onPageChange" />
        </div>
      </template>
    </Loading>
  </div>
</template>
