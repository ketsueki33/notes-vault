##### create an async function to connect to MongoDB (atlas)
after you have set up your cluster and added the `db_url` to `.env.local`, create a `mongoose.ts` file that exports a function to connect to the database.
```tsx title=mongoose.ts
import mongoose from "mongoose";

let isConnected: boolean = false;

export const connectToDatabase = async () => {
    mongoose.set("strictQuery", true);

    if (!process.env.MONGODB_URL)
        return console.log("Missing MongoDB URL.. please check");

    if (isConnected) return console.log("already connected to MongoDB");

    try {
        await mongoose.connect(process.env.MONGODB_URL, {
            dbName: "CodeOverflow",
        });

        isConnected = true;

        console.log("successfully connected to MongoDB");
    } catch (error) {
        console.log("connection to MongoDB failed", error);
    }
};
```

You can then call `connectTODatabase` function inside any [[Server Actions]] that requires connection to the database.
**Example:**
```ts
export async function editQuestion(params: EditQuestionParams) {
    try {
        connectToDatabase();

        const { questionId, title, content } = params;

        const question = await Question.findById(questionId);

        if (!question) throw new Error("Question not found");

        question.title = title;
        question.content = content;

        await question.save();
    } catch (error) {
        console.log(error);
        throw error;
    }
}
```
#### Defining the Models
We will define a User model as an example. It is good practice to keep all your db Models together. Eg) user model could in the file `user.model.ts` in the folder `database`.

**1.  Create a TypeScript interface for the schema**
We will  first create a TypeScript schema so that we know which fields exist. It extends the `Document`object from `mongoose` as  this will also give us access to the default properties and methods that are available in a MongoDB model.
```ts
export interface IUser {
    name: string;
    username: string;
    email: string;
    password: string;
    bio?: string;
    picture: string;
    saved: Schema.Types.ObjectId[]; // to reference another Model
    joinedAt: Date;
}
```

**2. Create a Schema for the database**
Define the schema which have the same fields and types as the interface. 
```ts
const UserSchema = new Schema({
    name: { type: String, required: true },
    username: { type: String, required: true, unique: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    bio: { type: String },
    picture: { type: String, required: true },
    saved: [{ type: Schema.Types.ObjectId, ref: "Question" }],
    joinedAt: { type: Date, default: Date.now }
});
```

**3. Initialize the Model with the schema**
We will first check if the `User` is already initialized. If it is not, we will initialize a new instance of the`User` model.
```ts
const User = models.User || model("User", UserSchema);
```

```ts title="database/user.model.ts"
import { Schema, models, model, Document } from "mongoose";

export interface IUser {
    name: string;
    username: string;
    email: string;
    password: string;
    bio?: string;
    picture: string;
    saved: Schema.Types.ObjectId[]; // to reference another Model
    joinedAt: Date;
}

const UserSchema = new Schema({
    name: { type: String, required: true },
    username: { type: String, required: true, unique: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    bio: { type: String },
    picture: { type: String, required: true },
    saved: [{ type: Schema.Types.ObjectId, ref: "Question" }],
    joinedAt: { type: Date, default: Date.now }
});

const User = models.User || model("User", UserSchema);

export default User;
```

#### Defining Types when Populating
Sometimes where we are querying a collection that has one field as a reference to another collection.. we might populate it or some fields of it.. in this case we can define a new type for a specific query using the existing TypeScript interface for the schema.

Eg)
```ts
export interface PopulatedQuestionById
    extends Omit<IQuestion, "author" | "tags"> {
    author: Pick<IUser, "_id" | "clerkId" | "username" | "picture">;
    tags: Pick<ITags, "_id" | "name">[];
}

export async function getQuestionById(
    params: GetQuestionByIdParams,
): Promise<PopulatedQuestionById> {
    try {
        connectToDatabase();
        const { questionId } = params;

        const question = await Question.findById(questionId)
            .populate({
                path: "tags",
                model: Tag,
                select: "name _id",
            })
            .populate({
                path: "author",
                model: User,
                select: "clerkId _id picture username",
            });

        return question;
    } catch (error) {
        console.log(error);
        throw error;
    }
}
```

