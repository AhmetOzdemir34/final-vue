<template>
	<div class="max-w-md p-5 bg-white rounded-md">
		<div class="flex flex-row justify-start">
			<div>
				<img
					src="../assets/Ellipse 1@2x.png"
					alt="no image"
					class="w-10 h-10 rounded-full"
				/>
			</div>
			<div class="flex-1 px-2">
				<p class="text-[8px] text-gray-400">
					{{ props.date }} - {{ props.time }}
				</p>
				<p
					v-if="!isEditable"
					class="text-sm"
				>
					{{ props.note }}
				</p>
				<input
					v-else
					type="text"
					v-model="updatedNote"
					class="w-full border border-slate-300 py-2 outline-none"
					:placeholder="props.note"
				/>
			</div>
		</div>
		<hr
			class="my-2"
			style="border-color: #eaecee"
		/>
		<div class="flex flex-row justify-between">
			<div class="flex gap-x-2">
				<div class="flex gap-x-2 items-center">
					<LikeIcon class="cursor-pointer" />
					<span style="color: #c1c8ce">{{
						props.like ? '1' : '0'
					}}</span>
				</div>
				<div class="flex gap-x-2 items-center">
					<DislikeIcon class="cursor-pointer" />
					<span style="color: #c1c8ce">{{
						props.dislike ? '1' : '0'
					}}</span>
				</div>
			</div>
			<div class="flex gap-x-2">
				<CheckIcon
					v-if="isEditable"
					class="cursor-pointer"
					@click="update"
				/>
				<EditIcon
					class="cursor-pointer"
					@click="changeEditable"
				/>
				<DeleteIcon
					class="cursor-pointer"
					@click="deleteItem(props.id)"
				/>
			</div>
		</div>
	</div>
</template>

<script setup>
import DeleteIcon from '../icons/DeleteIcon.vue';
import EditIcon from '../icons/EditIcon.vue';
import CheckIcon from '../icons/CheckIcon.vue';
import LikeIcon from '../icons/LikeIcon.vue';
import DislikeIcon from '../icons/DislikeIcon.vue';

import { ref } from 'vue';
const props = defineProps({
	id: Number,
	note: String,
	date: String,
	time: String,
	like: Boolean,
	dislike: Boolean
});

const emit = defineEmits(['delete-item', 'update-item']);

const updatedNote = ref('');
const isEditable = ref(false);

const deleteItem = (value) => {
	emit('delete-item', value);
};

const update = () => {
	emit('update-item', updatedNote.value, props.id);
	isEditable.value = false;
};

const changeEditable = () => {
	isEditable.value = !isEditable.value;
	updatedNote.value = '';
};
</script>

<style scoped></style>
