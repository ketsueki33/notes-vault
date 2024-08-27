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

You can then call `connectTODatabase` function inside any [[Server Actions]] that require connection to the database.

#### Defining the Models
We will define a User model as an example. It is good practice to keep all your db Models together. Eg) user model could in the file `user.model.ts` in the folder `database`.

**1.  Create a TypeScript interface for the schema**
We will  first create a TypeScript schema so that we know which fields exist. It extends the `Document`object from `mongoose` as  this will also give us access to the default properties and methods that are available in a MongoDB model.
```ts
export interface IUser extends Document {
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

export interface IUser extends Document {
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

