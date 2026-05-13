<script setup lang="ts">
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";
import type { TypedDocumentNode } from "@graphql-typed-document-node/core";
import { computed } from "vue";

const QUERY: TypedDocumentNode<
  {
    characters: { results: { id: string; name: string }[] };
  },
  Record<string, never>
> = gql`
  query Characters {
    characters {
      results {
        id
        name
      }
    }
  }
`;

const { result, loading, error } = useQuery(QUERY, {
  enabled: computed(() => true),
});
</script>

<template>
  <div v-if="loading">Loading...</div>
  <div v-else-if="error">Error: {{ error.message }}</div>
  <div v-else-if="result">
    {{
      result.characters.results.map((character) => character.name).join(", ")
    }}
  </div>
</template>
